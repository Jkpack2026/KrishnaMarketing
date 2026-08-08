name: Build Krishna Marketing APK

on:
  workflow_dispatch:
  push:
    paths:
      - "KrishnaMarketing_App_Part7.zip"
      - ".github/workflows/main.yml"

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Extract Part 7
        run: |
          mkdir app-project
          unzip -q KrishnaMarketing_App_Part7.zip -d app-project
          echo "Project files:"
          find app-project -maxdepth 3 -type f

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: "17"

      - name: Setup Android SDK
        uses: android-actions/setup-android@v3

      - name: Install Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: "8.7"

      - name: Build Debug APK
        working-directory: app-project
        run: |
          gradle assembleDebug --no-daemon

      - name: Find APK
        run: |
          find app-project/app/build/outputs/apk -type f -name "*.apk" -print

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: KrishnaMarketing-debug-apk
          path: app-project/app/build/outputs/apk/debug/*.apk
