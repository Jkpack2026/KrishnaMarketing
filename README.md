name: Build Krishna Marketing APK

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Extract Part 7
        run: |
          mkdir -p app-project
          unzip -o KrishnaMarketing_App_Part7.zip -d app-project

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Setup Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Build Debug APK
        working-directory: app-project
        run: gradle assembleDebug --stacktrace

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Krishna-Marketing-debug-APK
          path: app-project/app/build/outputs/apk/debug/app-debug.apk
          
