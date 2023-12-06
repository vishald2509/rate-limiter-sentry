# Rate Limiter NPM Package

## Overview

The `rate-limiter` npm package is a powerful and flexible rate-limiting middleware designed for microservices and distributed systems. It supports various data sources, such as Node cache and Redis, and can be seamlessly integrated into any Node.js library. The primary goal is to prevent resource starvation, improving the availability of API-based services by mitigating unintentional load-based denial-of-service incidents.

## Objective :star:

Prevent resource starvation and improve the availability of API-based services by enforcing rate limits, ensuring fair resource utilization.

## Success Criteria :large_blue_diamond:

Ensure that no IP address can make requests beyond the permitted limit within the defined interval window.

## Features :large_blue_diamond:

- **IP-Based Rate Limiting:** Restricts the number of requests from a specific IP address within a defined time window.

- **App-level Rate Limiting:** Limits the total number of requests for the entire application, irrespective of the IP address.

- **Easy Setup:** Simple configuration and setup using Redis credentials. Quick integration through middleware.

## Quick Start :large_blue_diamond:

1. Install the package:

   ```bash
   npm install rate-limiter@1.0.1
   ```

2. Set the environment variable for rate limiting:

   ```bash
   RATE_LIMIT_HEADER=true
   ```

3. Add the middleware to your server:

   ```javascript
   const initRateLimit = require('rate-limiter');
   const rateLimitConfig = require('./rateLimitConfig');

   app.use(
     '/your-api-endpoint',
     await initRateLimit(rateLimitConfig, { ...listeners })
   );
   ```

## Configuration :gear:

Modify the `environment.js` and `rateLimitConfig.js` files based on your application's requirements.

### `environment.js`:

```javascript
module.exports = {
  appName: 'your-app',
  redis: {
    port: process.env.REDIS_PORT || null,
    uri: process.env.REDIS_URI || null,
  },
  rateLimit: {
    // ...other configurations
  },
  isRateLimitEnabled: process.env.RATE_LIMIT_ENABLED || false,
};
```

### `rateLimitConfig.js`:

```javascript
const {
  appName,
  redis,
  rateLimit: rateLimitConfig,
  isRateLimitEnabled,
} = require('./environment');

module.exports = {
  appName,
  databaseConfig: {
    database: 'redis', // accepts 'redis' or 'nodecache'
    clusterMode: true, // boolean if redis is in cluster mode
    credentials: [
      {
        port: redis.port,
        host: redis.uri,
      },
    ],
  },
  rateLimitConfig,
  isRateLimitEnabled,
};
```

## Error Debugging :bug:

To aid in debugging, set the `RATE_LIMIT_HEADER=true` environment variable for detailed response headers. Analyze the headers:

- `x-ratelimit-limit`: Max request hits
- `x-ratelimit-remaining`: Number of requests remaining
- `x-ratelimit-reset`: Remaining window time for reset in seconds.

## Recommendations :dart:

For development environments, set `RATE_LIMIT_HEADER=true`. Adjust other configurations based on your application's needs.

## Log on Successful Implementation :white_check_mark:

Upon successful implementation, log network calls to the application backend with the specified response headers.

## Notification :email:

Once the implementation is completed, notify the respective team for acknowledgment.

Feel free to reach out if you encounter any issues or need further assistance. Happy coding! 🚀