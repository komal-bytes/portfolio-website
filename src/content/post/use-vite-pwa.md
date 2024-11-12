---
layout: ../../layouts/post.astro
title: Turning Your Vite App into a Progressive Web App (PWA)
description: In this guide, learn how to transform your app into a powerful PWA with offline support, caching strategies, and background sync. Perfect for developers aiming to boost app performance and reliability—whether your users are connected or not!"
dateFormatted: Nov 12th, 2024
---

<div class="my-1"></div>

If you're looking to take your web app to the next level, turning it into a Progressive Web App (PWA) is a fantastic option. PWAs allow your app to work offline, be installed on mobile devices, and provide a smooth, app-like experience. In this guide, we'll walk through the essentials of how to enable PWA features in your Vite app using the Vite PWA plugin.

## What is a PWA?

A Progressive Web App (PWA) is a type of web app that uses modern web technologies to deliver a native-like experience to users, with offline support, fast loading, and the ability to be installed on a device’s home screen. PWAs work on any platform that uses a standards-compliant browser, making them more accessible and versatile than traditional web apps.

## What is a Service Worker and Workbox?

At the heart of PWAs is the service worker. A service worker is a JavaScript file that runs in the background, separate from the web page, and can intercept network requests, cache responses, and manage offline behavior. This enables features like offline functionality, background sync, and push notifications.

Workbox is a set of libraries from Google that makes it easier to work with service workers. It provides caching strategies, background sync, and more, without requiring you to write all the code from scratch.

## Types of Workbox Modes: `generateSW` vs `injectManifest`

In Workbox, there are two main modes for generating the service worker:

- **generateSW**: Automatically generates the service worker, along with the caching strategies and precache manifest. It's an excellent choice for most apps that just need a simple caching setup.
  
- **injectManifest**: This mode is more flexible and allows you to write your own service worker. It’s perfect if you need more control over the service worker’s behavior, like custom caching strategies or additional features.

In this guide, we will focus on `generateSW`, as it’s the easiest way to get started with PWAs in Vite.

## Plugin Configuration in `vite.config.ts`

The VitePWA plugin simplifies the process of adding PWA capabilities to your app. Here’s how you can configure it in your `vite.config.ts` file:

```ts
import VitePWA from 'vite-plugin-pwa';

export default {
  plugins: [
    VitePWA({
      registerType: 'autoUpdate', // Automatically updates the service worker
      includeAssets: ['index.html', 'icons/*.png', 'robots.txt'],
      manifest: {
        name: 'My PWA App',
        short_name: 'PWA',
        start_url: '/',
        display: 'standalone',
        description: 'A simple PWA app built with Vite',
        theme_color: '#ffffff',
        icons: [
          {
            src: '/icon-192x192.png',
            sizes: '192x192',
            type: 'image/png',
          },
          {
            src: '/icon-512x512.png',
            sizes: '512x512',
            type: 'image/png',
          }
        ]
      },
      workbox: {
        cleanupOutdatedCaches: true,
        globPatterns: ["**/*.{mjs,js,ts,css,html,png,svg,ico,jpeg,jpg,json}"],
      },
    })
  ]
}

```


### Key Points:
- **registerType: 'autoUpdate'**: Ensures that the service worker is automatically updated when new versions are available.
- **includeAssets**: Specifies which assets should be cached and included in the PWA (like images, HTML, and robots.txt).
- **manifest**: Defines metadata for your app (e.g., name, icons, theme color).
- **workbox**: Contains caching strategies. Here, we enable cleanup of outdated caches and define file patterns to cache.

## Changes in `index.html` for Service Workers with Vite PWA Plugin

Include the PWA manifest link in the `<head>` section so that browsers know how to configure your app for PWA features.

```html
<head>
  <!-- Other head content -->
  
  <!-- Add manifest link for PWA -->
  <link rel="manifest" href="/manifest.webmanifest">
</head>
```

## Precaching with Workbox
One of the core features of a PWA is precaching. When you register a service worker, the browser installs it and caches the assets specified in the precache manifest. This ensures that users can continue to use your app even when they're offline.

### How It Works:
- **Service Worker Installation**: When the service worker is installed, it fetches and caches the essential resources (like JavaScript, CSS, images, etc.).
- **Activation**: After caching, the service worker takes control, allowing the app to run offline by serving cached files when there's no internet connection.

With the configuration above, Workbox automatically manages precaching, ensuring that your assets are stored efficiently.

## Generated Files in Build
When you run `vite build`, the following files are generated in your public directory:

- **sw.js**: The service worker file that handles caching and offline support.
- **workbox-*.js**: Workbox libraries that handle caching strategies and network requests.
- **manifest.webmanifest**: A file containing metadata about your app (e.g., icons, theme color) required for PWA installation.

## Enabling Service Worker in Dev Mode
Vite PWA also supports enabling the service worker during development, so you can test the offline capabilities while building your app. To enable the service worker in dev mode, you can set the `devOptions` in the plugin configuration:

```ts
VitePWA({
  devOptions: {
    enabled: true, // Enable service worker in dev mode
    type: 'module', // Use ES module service worker
  },
})

```

This allows you to test service worker features like caching and offline functionality during development without having to build your app first.

## Network-First Strategy
The **Network-First** strategy ensures that the app tries to fetch content from the network first and falls back to the cache if the network is unavailable. This is particularly useful for dynamic content or API calls that should be up-to-date.

To use the **Network-First** strategy in Vite PWA with `generateSW`, configure the plugin like this:

```ts
VitePWA({
  workbox: {
    runtimeCaching: [
      {
        urlPattern: /api\/.*\/*.json/,
        handler: 'NetworkFirst', // Use network-first strategy for API calls
        options: {
          networkTimeoutSeconds: 10, // Fallback to cache if network is slow
        },
      },
    ],
  },
})
```

This ensures that API requests are always fetched from the network, but if the network fails, it will fall back to the cached response.

## Background Sync
**Background Sync** allows your app to send data to the server even when the user is offline. This is useful for apps that need to sync user-generated content, like forms or comments.

To implement **Background Sync**, configure the plugin like this:

```ts
VitePWA({
  workbox: {
    runtimeCaching: [
      {
        urlPattern: /api\/.*\/*.json/,
        handler: 'NetworkFirst',
        options: {
        backgroundSync: {
          name: 'myQueueName',
          options: {
            maxRetentionTime: 24 * 60
          }
        }
      }
      },
    ],
  },
})
```

When the user is offline and submits a request (like filling out a form), it will be queued and sent once the network is available again.

## Wrapping Up

In this guide, we've covered how to use Vite PWA to turn your app into a Progressive Web App. We walked through the configuration process, how precaching works, enabling the service worker in dev mode, and implementing Network-First caching and Background Sync for improved offline support.

With Vite PWA, you can make your app more reliable, performant, and engaging—whether your users are online or offline. Happy coding!


