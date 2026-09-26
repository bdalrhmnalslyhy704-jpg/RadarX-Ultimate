name: Build Android APK

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-24.04

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '21'
          cache: gradle

      - name: Grant execute permission for Gradle
        working-directory: android
        run: chmod +x gradlew

      - name: Build Debug APK
        working-directory: android
        run: ./gradlew assembleDebug --stacktrace

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: RadarX-Ultimate-Debug-APK
          path: android/app/build/outputs/apk/debug/app-debug.apk
          if-no-files-found: error
