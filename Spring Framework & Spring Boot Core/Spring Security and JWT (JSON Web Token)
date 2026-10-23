# Spring Security and JWT (JSON Web Token)

## 1. Overview & Architecture
Spring Security is a powerful and highly customizable authentication and access-control framework. In modern microservices and REST APIs, it is typically configured as **stateless** using **JWT (JSON Web Token)** instead of traditional HTTP session-based cookies.

### Core Concepts
* **Authentication**: Verifying *who* a user is (e.g., username/password validation).
* **Authorization**: Verifying *what* roles or permissions a validated user has access to.
* **Security Filter Chain**: A chain of servlet filters that intercepts incoming HTTP requests to perform authentication, authorization, CSRF protection, and exception handling.

---

## 2. Stateless Authentication Flow with JWT
1. **Login Request**: Client sends credentials (`username` and `password`) to `/api/auth/login`.
2. **Authentication Manager**: Spring Security validates the credentials via `UserDetailsService` and `PasswordEncoder`.
3. **Token Generation**: Upon successful authentication, the server generates a signed **JWT** containing claims (username, roles) and returns it in the response.
4. **Subsequent Requests**: Client sends the JWT in the HTTP `Authorization` header (`Bearer <token>`).
5. **JWT Filter**: A custom filter intercepts the request, extracts the JWT, validates its signature and expiration, sets the `Authentication` object in the `SecurityContext`, and allows the request down the filter chain.

---

## 3. Spring Security Configuration (Spring Boot 3 / Security 6+)
In modern Spring Security, configuration is done via component-based security filter chain beans instead of extending `WebSecurityConfigurerAdapter` (which is deprecated).

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
@EnableMethodSecurity // Enables @PreAuthorize / @Secured method-level security
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthFilter;
    private final UserDetailsService userDetailsService;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthFilter, UserDetailsService userDetailsService) {
        this.jwtAuthFilter = jwtAuthFilter;
        this.userDetailsService = userDetailsService;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable()) // Disable CSRF for stateless REST APIs
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll() // Public endpoints
                .requestMatchers("/api/admin/**").hasRole("ADMIN") // Role-based endpoint
                .anyRequest().authenticated() // All other requests require authentication
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS) // Stateless session
            )
            .authenticationProvider(authenticationProvider())
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    @Bean
    public AuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
        authProvider.setUserDetailsService(userDetailsService);
        authProvider.setPasswordEncoder(passwordEncoder());
        return authProvider;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder(); // Secure hashing algorithm
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }
}
```

---

## 4. JWT Utility & Custom Filter Implementation

### JWT Token Utility (Generation & Validation)
```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.io.Decoders;
import io.jsonwebtoken.security.Keys;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Component;
import java.security.Key;
import java.util.Date;
import java.util.function.Function;

@Component
public class JwtService {

    // Secret key should be securely stored in application properties or environment variables
    private static final String SECRET_KEY = "404E635266556A586E3272357538782F413F4428472B4B6250645367566B5970";

    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = extractAllClaims(token);
        return claimsResolver.apply(claims);
    }

    public String generateToken(UserDetails userDetails) {
        return Jwts.builder()
                .setSubject(userDetails.getUsername())
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 10)) // 10 hours
                .signWith(getSignInKey(), SignatureAlgorithm.HS256)
                .compact();
    }

    public boolean isTokenValid(String token, UserDetails userDetails) {
        final String username = extractUsername(token);
        return (username.equals(userDetails.getUsername())) && !isTokenExpired(token);
    }

    private boolean isTokenExpired(String token) {
        return extractExpiration(token).before(new Date());
    }

    private Date extractExpiration(String token) {
        return extractClaim(token, Claims::getExpiration);
    }

    private Claims extractAllClaims(String token) {
        return Jwts.parserBuilder()
                .setSigningKey(getSignInKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
    }

    private Key getSignInKey() {
        byte[] keyBytes = Decoders.BASE64.decode(SECRET_KEY);
        return Keys.hmacShaKeyFor(keyBytes);
    }
}
```

### Custom JWT Authentication Filter
```java
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;
import java.io.IOException;

@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    public JwtAuthenticationFilter(JwtService jwtService, UserDetailsService userDetailsService) {
        this.jwtService = jwtService;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {
        final String authHeader = request.getHeader("Authorization");
        final String jwt;
        final String userEmail;

        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response);
            return;
        }

        jwt = authHeader.substring(7);
        userEmail = jwtService.extractUsername(jwt);

        if (userEmail != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = this.userDetailsService.loadUserByUsername(userEmail);
            if (jwtService.isTokenValid(jwt, userDetails)) {
                UsernamePasswordAuthenticationToken authToken = new UsernamePasswordAuthenticationToken(
                        userDetails,
                        null,
                        userDetails.getAuthorities()
                );
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }
        filterChain.doFilter(request, response);
    }
}
```

---

## 5. Tricky Interview Questions & Answers

### Q1. Why do we set `SessionCreationPolicy.STATELESS` when using JWT?
JWT is self-contained; all user identity and permission details are stored inside the cryptographically signed token. Since the server does not need to maintain active HTTP sessions or store authentication state in server memory (HttpSession), setting it to `STATELESS` ensures that Spring Security scales horizontally across multiple instances without sticky sessions.

### Q2. How do you handle JWT token expiration and user logout in a stateless architecture?
* **Expiration**: Handled automatically by checking the `exp` claim inside the token payload. If expired, the token parser throws an exception, resulting in a `401 Unauthorized` response.
* **Logout**: Because JWT is stateless, the server cannot invalidate a token directly once issued. Common strategies include:
  1. Short token expiration times combined with **Refresh Tokens**.
  2. Maintaining a server-side blacklist (e.g., in Redis) of revoked/logged-out JWT IDs (`jti`).

### Q3. What is the difference between `@PreAuthorize`, `@Secured`, and configuration-based `.requestMatchers()`?
* **`.requestMatchers()`**: URL-based security configured in the `SecurityFilterChain`. Good for securing broad endpoint prefixes (e.g., `/api/admin/**`).
* **`@PreAuthorize`**: Expression-based method security evaluated *before* entering a method (e.g., `@PreAuthorize("hasRole('ADMIN')")`). Supports SpEL (Spring Expression Language) for complex dynamic authorization checks based on method arguments.
* **`@Secured`**: Legacy role-based annotation (requires exact role string matching like `ROLE_ADMIN`). Generally superseded by `@PreAuthorize`.

---

## 6. Quick Revision
* **Authentication vs Authorization**: Authentication verifies identity; authorization checks permissions/roles.
* **Stateless Security**: Uses `SessionCreationPolicy.STATELESS` to eliminate server-side session overhead.
* **Filter Chain**: Intercepts requests via custom filters like `JwtAuthenticationFilter` before reaching controllers.
* **BCrypt**: Industry-standard, salted hashing algorithm used via `PasswordEncoder` for securely storing passwords.
* **JWT Claims & Signature**: Contains signed metadata (username, expiration) verified against a secret key on every request.
