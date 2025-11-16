name: Android CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:               # 👈 显式授权
      contents: write

    steps:
      - uses: actions/checkout@v4

      - name: set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: gradle

      - name: Grant execute permission for gradlew
        run: chmod +x gradlew

      - name: Build Release APK
        run: ./gradlew assembleRelease

      # 只在 push 时发版；PR 时跳过
      - name: Release
        if: github.event_name == 'push'
        uses: softprops/action-gh-release@v2
        with:
          tag_name: v1.0.${{ github.run_number }}
          name: Release v1.0.${{ github.run_number }}
          files: app/build/outputs/apk/release/app-release.apk
          generate_release_notes: true
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
