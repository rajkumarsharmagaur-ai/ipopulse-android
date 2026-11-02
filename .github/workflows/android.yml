name: Build IPOPulse APK

on:
  push:
    branches: [ "main", "master" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: 1. Checkout Repository
        uses: actions/checkout@v4

      - name: 2. Auto-Extract ZIP File
        run: |
          for z in *.zip; do
            if [ -f "$z" ]; then
              echo "Extracting $z..."
              unzip -q -o "$z"
            fi
          done

      - name: 3. Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: 4. Set up Gradle 8.5
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.5'

      - name: 5. Build Debug APK
        run: |
          if [ -f "settings.gradle" ]; then
            gradle assembleDebug --no-daemon
          elif [ -d "android-kotlin-compose" ]; then
            cd android-kotlin-compose && gradle assembleDebug --no-daemon
          fi

      - name: 6. Upload Ready APK
        uses: actions/upload-artifact@v4
        with:
          name: IPOPulse-Installable-APK
          path: "**/build/outputs/apk/debug/*.apk"
