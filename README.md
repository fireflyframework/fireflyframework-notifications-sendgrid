# Firefly Framework - Notifications - SendGrid

[![CI](https://github.com/fireflyframework/fireflyframework-notifications-sendgrid/actions/workflows/ci.yml/badge.svg)](https://github.com/fireflyframework/fireflyframework-notifications-sendgrid/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-21%2B-orange.svg)](https://openjdk.org)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-green.svg)](https://spring.io/projects/spring-boot)

> SendGrid email adapter for Firefly Notifications.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

## Overview

Firefly Framework Notifications SendGrid implements the `EmailProvider` interface from the Firefly Notifications core module using SendGrid as the delivery provider. It provides `SendGridEmailProvider` which handles email delivery through the SendGrid API.

The module includes auto-configuration for seamless activation when included on the classpath alongside the notifications core module. Configuration properties allow customizing API credentials and provider-specific settings.

## Features

- `EmailProvider` implementation using SendGrid
- Spring Boot auto-configuration for seamless activation
- Configurable API credentials via application properties
- Standalone provider library (include alongside fireflyframework-notifications)

## Requirements

- Java 21+
- Spring Boot 3.x
- Maven 3.9+
- SendGrid account and API credentials

## Installation

```xml
<dependency>
    <groupId>org.fireflyframework</groupId>
    <artifactId>fireflyframework-notifications-sendgrid</artifactId>
    <version>26.02.03</version>
</dependency>
```

## Quick Start

```xml
<dependencies>
    <dependency>
        <groupId>org.fireflyframework</groupId>
        <artifactId>fireflyframework-notifications</artifactId>
    </dependency>
    <dependency>
        <groupId>org.fireflyframework</groupId>
        <artifactId>fireflyframework-notifications-sendgrid</artifactId>
    </dependency>
</dependencies>
```

## Configuration

```yaml
firefly:
  notifications:
    sendgrid:
      api-key: SG.xxxxxxxxxx
      from-email: noreply@example.com
      from-name: My Application
```

## Documentation

No additional documentation available for this project.

## Contributing

Contributions are welcome. Please read the [CONTRIBUTING.md](CONTRIBUTING.md) guide for details on our code of conduct, development process, and how to submit pull requests.

## License

Copyright 2024-2026 Firefly Software Solutions Inc.

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
