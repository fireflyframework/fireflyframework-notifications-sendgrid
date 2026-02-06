# fireflyframework-notifications-sendgrid

SendGrid email adapter for Firefly Notifications Library.

## Overview

This module is an **infrastructure adapter** in the hexagonal architecture that implements the `EmailProvider` port interface. It handles all SendGrid-specific integration details, including API authentication, request transformation, and error handling.

### Architecture Role

```
Application Layer (EmailService)
    ↓ depends on
Domain Layer (EmailProvider interface)
    ↑ implemented by
Infrastructure Layer (SendGridEmailProvider) ← THIS MODULE
    ↓ calls
SendGrid REST API
```

This adapter can be swapped with other email providers (Resend, AWS SES) without changing your application code.

## Installation

Add this dependency to your `pom.xml`:

```xml path=null start=null
<dependency>
  <groupId>org.fireflyframework</groupId>
  <artifactId>fireflyframework-notifications-core</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>

<dependency>
  <groupId>org.fireflyframework</groupId>
  <artifactId>fireflyframework-notifications-sendgrid</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>
```

## Configuration

Add the following to your `application.yml`:

```yaml path=null start=null
notifications:
  email:
    provider: sendgrid  # Enables this adapter

sendgrid:
  api-key: ${SENDGRID_API_KEY}  # Your SendGrid API key
```

### Getting Your API Key

1. Sign up at [sendgrid.com](https://sendgrid.com)
2. Navigate to Settings → API Keys
3. Create a new API key with "Mail Send" permissions
4. Set it as an environment variable:
   ```bash
   export SENDGRID_API_KEY="SG.your-key-here"
   ```

## Usage

Inject `EmailService` from the core library. Spring automatically wires this adapter:

```java path=null start=null
@Service
public class NotificationService {
    
    @Autowired
    private EmailService emailService;
    
    public void sendWelcomeEmail(String recipient) {
        EmailRequestDTO request = EmailRequestDTO.builder()
            .from("noreply@example.com")
            .to(List.of(recipient))
            .subject("Welcome!")
            .html("<h1>Welcome to our platform</h1>")
            .text("Welcome to our platform")
            .build();
        
        emailService.sendEmail(request)
            .subscribe(response -> {
                if (response.isSuccess()) {
                    log.info("Email sent: {}", response.getMessageId());
                } else {
                    log.error("Failed: {}", response.getError());
                }
            });
    }
}
```

## Features

- **HTML and plain text** emails
- **CC and BCC** recipients
- **File attachments** with automatic base64 encoding
- **Reactive** - returns `Mono<EmailResponseDTO>`
- **Error handling** - Graceful degradation with error messages

### Sending with Attachments

```java path=null start=null
EmailAttachmentDTO attachment = EmailAttachmentDTO.builder()
    .filename("report.pdf")
    .content(pdfBytes)
    .contentType("application/pdf")
    .build();

EmailRequestDTO request = EmailRequestDTO.builder()
    .from("reports@example.com")
    .to(List.of("user@example.com"))
    .subject("Monthly Report")
    .html("<p>See attached report</p>")
    .attachments(List.of(attachment))
    .build();

emailService.sendEmail(request).subscribe();
```

## Switching Providers

To switch from SendGrid to another provider (e.g., Resend):

1. Remove this dependency from `pom.xml`
2. Add `fireflyframework-notifications-resend` dependency
3. Update configuration to use `provider: resend`

**No code changes required** in your services—that's the power of hexagonal architecture!

## Implementation Details

This adapter:
- Implements `EmailProvider` interface from `fireflyframework-notifications-core`
- Uses SendGrid Java SDK for API calls
- Transforms generic `EmailRequestDTO` to SendGrid's `Mail` object
- Handles authentication via API key
- Returns standardized `EmailResponseDTO`

## Troubleshooting

### Error: "No qualifying bean of type 'EmailProvider'"

- Ensure `notifications.email.provider=sendgrid` is set
- Verify `sendgrid.api-key` is configured

### Error: "Unauthorized"

- Check your API key is valid
- Verify the key has "Mail Send" permissions

## References

- [SendGrid API Documentation](https://docs.sendgrid.com/api-reference/mail-send/mail-send)
- [Firefly Notifications Architecture](../fireflyframework-notifications/ARCHITECTURE.md)
