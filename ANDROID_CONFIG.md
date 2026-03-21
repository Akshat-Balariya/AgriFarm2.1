# Android Client Configuration for Render Deployment

This file explains how to update your Android app to connect to Render-deployed servers.

## Current Configuration (Local Development)

**File**: `client/gradle.properties`

```properties
# Root Gradle settings used when building from AgriGo/.
org.gradle.jvmargs=-Xmx2048m -Dfile.encoding=UTF-8
kotlin.code.style=official

# Android app API endpoints (local development)
API_BASE_URL=http://10.0.2.2:5000
CHAT_API_BASE_URL=http://10.0.2.2:3000
```

## After Render Deployment

Once your server is deployed to Render, update to:

```properties
# Root Gradle settings used when building from AgriGo/.
org.gradle.jmvargs=-Xmx2048m -Dfile.encoding=UTF-8
kotlin.code.style=official

# Android app API endpoints (Render production)
API_BASE_URL=https://agrigo-api-server.onrender.com
CHAT_API_BASE_URL=https://agrigo-chatbot.onrender.com
```

## Explanation

- **Local Development**: `http://10.0.2.2:XXXX`
  - `10.0.2.2` is Android emulator's way to reach host machine
  - Good for testing with local servers
  - Not accessible outside your network

- **Render Production**: `https://agrigo-api-server.onrender.com`
  - Public HTTPS URLs
  - Accessible from anywhere (phone, emulator, etc.)
  - Automatically scaled and backed up

## Steps to Update

1. **After Render deployment**, you'll get URLs:
   - Flask API: `https://agrigo-api-server.onrender.com`
   - Chatbot: `https://agrigo-chatbot.onrender.com`

2. **Open `client/gradle.properties`**

3. **Replace the URLs**:
   ```diff
   - API_BASE_URL=http://10.0.2.2:5000
   + API_BASE_URL=https://agrigo-api-server.onrender.com
   
   - CHAT_API_BASE_URL=http://10.0.2.2:3000
   + CHAT_API_BASE_URL=https://agrigo-chatbot.onrender.com
   ```

4. **Rebuild and redeploy app**:
   ```bash
   cd client
   ./gradlew clean build
   # or use Android Studio's Build → Rebuild Project
   ```

## Build Variants (Optional)

For more control, you can create different build variants:

**`client/build.gradle.kts`**:
```kotlin
android {
    // ... other config ...
    
    buildTypes {
        debug {
            buildConfigField("String", "API_BASE_URL", "\"http://10.0.2.2:5000\"")
            buildConfigField("String", "CHAT_API_BASE_URL", "\"http://10.0.2.2:3000\"")
        }
        release {
            buildConfigField("String", "API_BASE_URL", "\"https://agrigo-api-server.onrender.com\"")
            buildConfigField("String", "CHAT_API_BASE_URL", "\"https://agrigo-chatbot.onrender.com\"")
        }
    }
}
```

Then in your Kotlin/Java code:
```kotlin
import com.example.agrigo.BuildConfig

val apiUrl = BuildConfig.API_BASE_URL  // Automatically switched
```

## Testing

After updating configuration:

1. **Rebuild the app**
   ```bash
   cd client
   ./gradlew clean assembleDebug
   ```

2. **Test endpoints**
   - Open app
   - Make API calls
   - Check Render logs: https://dashboard.render.com

3. **Verify in Render Dashboard**
   - Go to your service
   - Click "Logs"
   - See incoming requests from your app

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| "Cannot reach API" | Still using localhost | Update gradle.properties |
| "SSL certificate error" | Using HTTP instead of HTTPS | Use `https://` URLs |
| "404 Not Found" | Wrong endpoint path | Check exact endpoint in Render logs |
| "500 Server Error" | Server issue | Check Render logs for errors |
| "Connection timeout" | Render service down | Check Render dashboard status |

## Environment-Specific Configuration

**For Development**:
```properties
API_BASE_URL=http://10.0.2.2:5000
CHAT_API_BASE_URL=http://10.0.2.2:3000
DEBUG_LOGGING=true
```

**For Production**:
```properties
API_BASE_URL=https://agrigo-api-server.onrender.com
CHAT_API_BASE_URL=https://agrigo-chatbot.onrender.com
DEBUG_LOGGING=false
```

## CI/CD Integration (Optional)

If you use GitHub Actions for automated builds:

**`.github/workflows/android-build.yml`**:
```yaml
name: Android Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Build APK
        run: |
          cd client
          ./gradlew build
      - name: Upload to Play Store (optional)
        run: # Your deployment script
```

---

**Important**: After Render deployment, always update these URLs in your Android app configuration!

For Render URLs, check your deployed services:
- https://dashboard.render.com → Click on each service → Copy the URL

