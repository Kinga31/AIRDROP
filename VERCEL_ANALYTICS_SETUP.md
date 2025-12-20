# Vercel Web Analytics Setup Guide

This document describes how Vercel Web Analytics has been implemented in the AirdropAlpha project.

## Overview

Vercel Web Analytics is enabled for this project to track visitor metrics, page views, and user interactions. This helps us understand how users are engaging with the AirdropAlpha platform.

## Prerequisites Met

- ✅ Vercel account created
- ✅ Vercel project created (airdropalpha.vercel.app)
- ✅ Vercel CLI available for deployments

## Implementation for Static HTML Site

This project uses a static HTML implementation, which is the appropriate approach for plain HTML sites like AirdropAlpha.

### Analytics Script Configuration

The Vercel Web Analytics implementation consists of two main components added to `index.html`:

#### 1. Analytics Function Definition

Located in the `<head>` section of `index.html`:

```html
<!-- Vercel Web Analytics -->
<script>
  window.va = window.va || function () { (window.vaq = window.vaq || []).push(arguments); };
</script>
```

This creates the `window.va` function that queues analytics events until the analytics script loads.

#### 2. Analytics Script Tag

Also located in the `<head>` section:

```html
<script defer src="/_vercel/insights/script.js"></script>
```

This script is served by Vercel after deployment and handles all analytics tracking. The `defer` attribute ensures the script doesn't block page rendering.

## Project Files Modified/Created

### Files Modified

- **`index.html`**: Added Vercel Web Analytics script tags in the `<head>` section
  - Lines 231-233: Analytics implementation

- **`vercel.json`**: Configuration file for Vercel deployment
  - Includes redirect rules
  - Sets X-Robots-Tag header for search engines

- **`robots.txt`**: Search engine crawling instructions
  - Allows all crawlers
  - Points to sitemap.xml

- **`sitemap.xml`**: XML sitemap for SEO
  - Includes the root URL with daily change frequency

### Files Created

- **`favicon.ico`**: Website favicon (1.4 MB)
- **`googled6c9a6246be1992e.html`**: Google verification file
- **`.github/workflows/generator-generic-ossf-slsa3-publish.yml`**: GitHub Actions workflow

## How It Works

1. **Deployment**: When the site is deployed to Vercel, the analytics script becomes available at `/_vercel/insights/script.js`

2. **Script Loading**: When users visit the site, the `<script defer src="/_vercel/insights/script.js">` tag loads the analytics script

3. **Event Queuing**: While the script is loading, any analytics calls are queued via the `window.va` function

4. **Data Collection**: Once loaded, the script:
   - Tracks page views
   - Records visitor information
   - Monitors Core Web Vitals
   - Detects user interactions

5. **Data Sending**: Analytics data is sent to Vercel's servers via `/_vercel/insights/view` endpoints

## Viewing Analytics Data

After deployment and once users have visited the site:

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Select the **airdropalpha** project
3. Click the **Analytics** tab
4. View real-time visitor data, page views, and performance metrics

### Available Metrics

- **Page Views**: Number of times pages were viewed
- **Visitors**: Unique visitor counts
- **Core Web Vitals**: 
  - Largest Contentful Paint (LCP)
  - First Input Delay (FID)
  - Cumulative Layout Shift (CLS)
- **Real User Monitoring (RUM)**: Performance data from actual users

## Important Notes

### Route Support Limitation

⚠️ **Note:** With the static HTML implementation, there is **no route support** for analytics. The analytics will track all traffic as coming from the root `/` path since there are no actual routes in a static HTML site.

### No Custom Events

With the static HTML implementation, custom event tracking (like button clicks or form submissions) requires additional code. The basic setup only tracks:
- Page views
- Performance metrics
- Visitor information

To add custom events, you would need to use:

```javascript
// Example: Custom event tracking
window.va('event', {
  name: 'airdrop_clicked',
  data: {
    airdrop_name: 'ProjectName'
  }
});
```

### Privacy and Compliance

Vercel Web Analytics:
- ✅ GDPR compliant
- ✅ No cookies required
- ✅ No personal data collection
- ✅ Privacy-focused by design
- Meets CCPA requirements

Learn more: [Vercel Analytics Privacy Policy](https://vercel.com/analytics/privacy)

## Network Requests

When properly configured, you should see a Fetch/XHR request in your browser's Network tab from `/_vercel/insights/view` when visiting the site.

To verify:
1. Open Developer Tools (F12)
2. Go to the Network tab
3. Visit any page on the site
4. Look for requests to `/_vercel/insights/view`

## Deployment

The site is configured to be deployed to Vercel:

```bash
vercel deploy
```

Recommended: Connect your Git repository to Vercel for automatic deployments on each push to main.

## Configuration Files

### vercel.json

```json
{
  "redirects": [
    {
      "source": "/",
      "destination": "https://airdropalpha.vercel.app/",
      "permanent": true
    }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "X-Robots-Tag",
          "value": "all"
        }
      ]
    }
  ]
}
```

This configuration:
- Redirects root to the domain with HTTPS
- Sets robots header to allow all search engines to crawl

## Next Steps

1. **Verify Deployment**: Ensure the site is deployed to Vercel at airdropalpha.vercel.app
2. **Enable Web Analytics**: Visit Vercel Dashboard → Project → Analytics tab → Click Enable (if not already enabled)
3. **Monitor Data**: Check analytics dashboard for visitor metrics
4. **Set Up GitHub Integration**: Connect the repository for automatic deployments
5. **Consider Custom Events**: If you want to track specific user interactions, implement custom event tracking

## Troubleshooting

### Analytics Not Showing Data

1. **Check Deployment**: Verify the site is deployed to Vercel
2. **Verify Script Loading**: Check the Network tab for successful script loading
3. **Wait for Data**: Analytics data takes a few minutes to appear
4. **Check Browser Console**: Look for any errors in the console

### Script Loading Issues

If `/_vercel/insights/script.js` returns 404:
- Ensure the site is deployed to Vercel (not a different host)
- Check that Vercel Web Analytics is enabled in the project settings

## References

- [Vercel Web Analytics Documentation](https://vercel.com/docs/analytics)
- [Vercel Analytics Limits and Pricing](https://vercel.com/docs/analytics/limits-and-pricing)
- [Web Vitals Guide](https://web.dev/vitals/)
- [Vercel Privacy Policy](https://vercel.com/legal/analytics-privacy-policy)

## Summary

The AirdropAlpha project successfully implements Vercel Web Analytics using the static HTML approach, which is appropriate for this use case. The implementation is minimal, non-intrusive, and provides valuable insights into user behavior and site performance without requiring any additional dependencies or package installations.
