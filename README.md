name: Build Windows EXE

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install

      - name: Build project
        run: npm run build

      - name: Create dist folder
        run: mkdir -p dist

      - name: Package EXE
        run: |
          npm install --global pkg
          pkg . --targets node20-win-x64 --output dist/MyApp.exe

      - name: List dist contents
        run: dir dist

      - name: Upload EXE
        uses: actions/upload-artifact@v4
        with:
          name: MyApp-Windows
          path: dist/MyApp.exe
