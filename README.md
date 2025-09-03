# Cookie Cleaner - A Vue 3 Chrome Extension

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![VueJS](https://img.shields.io/badge/Vue.js-3-42b883.svg)
![Vite](https://img.shields.io/badge/Vite-5-646cff.svg)
![pnpm](https://img.shields.io/badge/pnpm-10-f69220.svg)

A simple browser extension built to clear all cookies for the current website's domain with a single click. This project serves as a comprehensive guide for creating modern Chrome extensions using Vue 3, Vite, TypeScript, and the CRXJS plugin.

## 📸 Screenshot

![Extension Popup](./screenshot.png)
_(Recommendation: Replace this with an actual screenshot of your extension's popup)_

---

## ✨ Features

- One-click clearing of all cookies for the active tab's domain.
- Simple, clean, and reactive user interface built with Vue 3.
- Provides clear feedback on the status of the operation (e.g., number of cookies deleted, or "no cookies found").
- Built with modern, fast development tools for a great developer experience.

## 🛠️ Tech Stack

- **Framework:** Vue 3 (using `<script setup>`)
- **Build Tool:** Vite
- **Extension Plugin:** CRXJS (`@crxjs/vite-plugin`)
- **Package Manager:** pnpm
- **Language:** TypeScript

---

## 🚀 Getting Started & Local Development

Follow these instructions to set up, run, and develop the project on your local machine.

### 1. Prerequisites

Ensure you have the following installed on your system:

- [Node.js](https://nodejs.org/) (LTS version recommended)
- [pnpm](https://pnpm.io/installation)

### 2. Installation

1.  Clone the repository to your local machine:
    ```bash
    git clone [https://github.com/username11-dev/vue-project-test-ext.git](https://github.com/username11-dev/vue-project-test-ext.git)
    ```
2.  Navigate into the project directory:
    ```bash
    cd vue-project-test-ext
    ```
3.  Install all the necessary packages using pnpm:
    ```bash
    pnpm install
    ```

### 3. Loading the Extension in Chrome (One-Time Setup)

1.  Open Google Chrome and navigate to `chrome://extensions`.
2.  Enable **"Developer mode"** using the toggle in the top-right corner.
3.  Run the build command once to generate the initial `dist` folder:
    ```bash
    pnpm build
    ```
4.  Click the **"Load unpacked"** button and select the `dist` folder located inside your project directory.
5.  The "Cookie Cleaner" extension will now appear in your list of extensions.

### 4. Development Workflow (Build-Based)

Due to potential stability issues with the Vite development server (HMR) in the extension environment, the most reliable workflow is to use the build command to see your changes.

The development cycle is as follows:

1.  Make your desired changes to the code in the `src` directory.
2.  Run the build command in your terminal. This updates the `dist` folder with your latest code.
    ```bash
    pnpm build
    ```
3.  Navigate back to `chrome://extensions` and click the **"Update"** button (🔄) on the extension's card.
4.  Test your changes by opening the extension's popup on any website.

---

## 📁 Key Project Files Explained

- **`manifest.json`**: The "passport" of the extension. It tells Chrome the extension's name, version, description, and what permissions it needs to function (`cookies`, `tabs`, etc.). The `action.default_popup` key points to `index.html`, which serves as the entry point for our Vue application.

- **`vite.config.ts`**: The main configuration file for Vite. Here, we register the necessary plugins (`vue()` and `crx({ manifest })`) that transform our source code into a functional extension. We also configure the development server here to resolve potential issues.

- **`src/App.vue`**: The "heart and soul" of the extension. This Single-File Component contains all the core logic and presentation:
  - **`<script setup>`**: All the TypeScript logic, including the functions that use the `chrome.cookies` and `chrome.tabs` APIs to find and remove cookies.
  - **`<template>`**: The HTML structure of the extension's popup interface.
  - **`<style scoped>`**: The CSS styles for the popup. The `scoped` attribute is crucial as it ensures these styles only apply to the extension and do not leak out and interfere with the websites the user visits.

## 📄 License

This project is licensed under the MIT License.
