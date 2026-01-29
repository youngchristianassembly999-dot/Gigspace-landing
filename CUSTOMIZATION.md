# Customization Guide for GigSpace Landing Page

This guide will help you customize the GigSpace landing page to match your brand and requirements.

## Table of Contents
- [Color Scheme](#color-scheme)
- [Typography](#typography)
- [Content Editing](#content-editing)
- [Adding Sections](#adding-sections)
- [Modifying Layout](#modifying-layout)
- [Custom Animations](#custom-animations)
- [Form Customization](#form-customization)
- [Advanced Customization](#advanced-customization)

---

## Color Scheme

### Changing Primary Colors

Edit the CSS variables in `styles.css`:

```css
:root {
    /* Change primary color (currently blue #0066FF) */
    --color-primary: #YOUR_COLOR;
    --color-primary-dark: #DARKER_SHADE;
    --color-primary-light: #LIGHTER_SHADE;
    
    /* Change secondary color (currently orange #FF6B35) */
    --color-secondary: #YOUR_COLOR;
    --color-secondary-dark: #DARKER_SHADE;
    --color-secondary-light: #LIGHTER_SHADE;
}
```

### Creating Custom Gradients

```css
:root {
    --gradient-primary: linear-gradient(135deg, #START_COLOR 0%, #END_COLOR 100%);
    --gradient-secondary: linear-gradient(135deg, #START_COLOR 0%, #END_COLOR 100%);
    --gradient-cta: linear-gradient(135deg, #START_COLOR 0%, #END_COLOR 100%);
}
```

### Dark Mode (Optional)

Add dark mode support:

```css
/* Add to styles.css */
@media (prefers-color-scheme: dark) {
    :root {
        --color-white: #1A1A1A;
        --color-black: #FFFFFF;
        --color-gray-50: #2A2A2A;
        --color-gray-900: #E8E8E8;
    }
}
```

---

## Typography

### Changing Fonts

1. **Choose your fonts** from [Google Fonts](https://fonts.google.com/)

2. **Update the font link** in `index.html`:
   ```html
   <link href="https://fonts.googleapis.com/css2?family=Your+Display+Font:wght@900&family=Your+Body+Font:wght@400;500;700&display=swap" rel="stylesheet">
   ```

3. **Update CSS variables** in `styles.css`:
   ```css
   :root {
       --font-display: 'Your Display Font', sans-serif;
       --font-body: 'Your Body Font', sans-serif;
   }
   ```

### Font Sizes

Adjust responsive font sizes:

```css
.hero-title-line {
    /* Adjust min, preferred, and max sizes */
    font-size: clamp(2.5rem, 8vw, 5rem);
}

.section-title {
    font-size: clamp(2rem, 5vw, 3.5rem);
}
```

---

## Content Editing

### Using the CMS

1. Access at `/admin` on your deployed site
2. Login with Netlify Identity
3. Edit content through the visual interface

### Manual Editing

Edit `index.html` directly for quick changes:

#### Hero Section
```html
<h1 class="hero-title">
    <span class="hero-title-line">Your New</span>
    <span class="hero-title-line">Headline</span>
    <span class="hero-title-line hero-title-accent">Goes Here</span>
</h1>
```

#### Services
Find the services section and edit each card:
```html
<div class="service-card">
    <div class="service-icon">
        <!-- SVG icon here -->
    </div>
    <h3 class="service-title">Your Service Name</h3>
    <p class="service-description">Your description</p>
    <div class="service-price">Your Price</div>
</div>
```

#### Pricing Tiers
```html
<div class="pricing-card">
    <div class="pricing-header">
        <h3 class="pricing-title">Your Plan Name</h3>
        <div class="pricing-price">
            <span class="pricing-amount">$XX</span>
            <span class="pricing-period">/month</span>
        </div>
    </div>
    <!-- Add features in list -->
</div>
```

---

## Adding Sections

### Adding a New Section

1. **Create HTML structure**:
```html
<section class="your-section" id="your-section">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Section Title</h2>
            <p class="section-subtitle">Section subtitle</p>
        </div>
        
        <!-- Your content here -->
    </div>
</section>
```

2. **Add to navigation**:
```html
<li><a href="#your-section" class="nav-link">Your Section</a></li>
```

3. **Style your section** in `styles.css`:
```css
.your-section {
    padding: var(--spacing-xl) 0;
    background: var(--color-gray-50);
}
```

### Pre-built Section Templates

#### Feature Grid Section
```html
<section class="features">
    <div class="container">
        <div class="features-grid">
            <div class="feature-item">
                <div class="feature-icon">🚀</div>
                <h3>Feature Title</h3>
                <p>Feature description</p>
            </div>
            <!-- Add more feature items -->
        </div>
    </div>
</section>
```

```css
.features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: var(--spacing-md);
}

.feature-item {
    text-align: center;
    padding: var(--spacing-md);
}
```

---

## Modifying Layout

### Changing Grid Columns

```css
/* From 3 columns to 4 columns */
.services-grid {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}

/* Fixed 2-column layout */
.contact-grid {
    grid-template-columns: 1fr 1fr;
}
```

### Adjusting Spacing

```css
:root {
    --spacing-xs: 0.5rem;   /* Increase for more space */
    --spacing-sm: 1rem;
    --spacing-md: 2rem;
    --spacing-lg: 4rem;
    --spacing-xl: 6rem;
}
```

### Container Width

```css
.container {
    max-width: 1400px; /* Increase for wider layout */
}
```

---

## Custom Animations

### Add Fade-In Animation to Elements

1. **Add class to element** in HTML:
```html
<div class="service-card fade-in">...</div>
```

2. **Animation is automatic** via JavaScript intersection observer

### Custom Animation Examples

#### Slide-In from Left
```css
@keyframes slideInLeft {
    from {
        opacity: 0;
        transform: translateX(-50px);
    }
    to {
        opacity: 1;
        transform: translateX(0);
    }
}

.slide-left {
    animation: slideInLeft 0.6s ease;
}
```

#### Bounce Effect
```css
@keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-10px); }
}

.bounce-element {
    animation: bounce 2s infinite;
}
```

### Changing Animation Speed

```css
:root {
    --transition-fast: 0.15s ease;  /* Faster */
    --transition-base: 0.3s ease;
    --transition-slow: 0.5s ease;   /* Slower */
}
```

---

## Form Customization

### Adding Form Fields

```html
<div class="form-group">
    <label for="new-field">New Field Label</label>
    <input type="text" id="new-field" name="new_field" required>
</div>
```

### Custom Form Validation

Add to `script.js`:

```javascript
// Custom validation for specific field
const phoneField = document.getElementById('phone');
if (phoneField) {
    phoneField.addEventListener('input', (e) => {
        const phoneRegex = /^\+?[\d\s-()]+$/;
        if (!phoneRegex.test(e.target.value)) {
            e.target.setCustomValidity('Please enter a valid phone number');
        } else {
            e.target.setCustomValidity('');
        }
    });
}
```

### Changing Form Action

For different form providers:

**EmailJS**:
```javascript
// Replace Formspree with EmailJS
emailjs.sendForm('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', form)
    .then(() => {
        showFormMessage('success', 'Email sent!');
    });
```

**Custom Backend**:
```javascript
fetch('https://your-api.com/contact', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(formData)
})
```

---

## Advanced Customization

### Adding Video Background

```html
<section class="hero">
    <video class="hero-video" autoplay muted loop>
        <source src="video/hero-bg.mp4" type="video/mp4">
    </video>
    <!-- Rest of hero content -->
</section>
```

```css
.hero-video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    z-index: -1;
    opacity: 0.3;
}
```

### Particle Background Effect

Using particles.js:

1. Add to `index.html` before `</body>`:
```html
<script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
```

2. Add container:
```html
<div id="particles-js"></div>
```

3. Initialize in `script.js`:
```javascript
particlesJS('particles-js', {
    particles: {
        number: { value: 80 },
        color: { value: '#0066FF' },
        shape: { type: 'circle' },
        opacity: { value: 0.5 },
        size: { value: 3 }
    }
});
```

### Custom Cursor

```css
body {
    cursor: url('images/cursor.png'), auto;
}

a, button {
    cursor: url('images/cursor-pointer.png'), pointer;
}
```

### Scroll Progress Indicator

Add to HTML:
```html
<div class="scroll-progress"></div>
```

Add to CSS:
```css
.scroll-progress {
    position: fixed;
    top: 0;
    left: 0;
    height: 4px;
    background: var(--gradient-primary);
    z-index: 9999;
    transition: width 0.1s;
}
```

Add to JavaScript:
```javascript
window.addEventListener('scroll', () => {
    const scrolled = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;
    document.querySelector('.scroll-progress').style.width = scrolled + '%';
});
```

### Loading Screen

```html
<div class="loader">
    <div class="loader-spinner"></div>
</div>
```

```css
.loader {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: var(--color-white);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10000;
    transition: opacity 0.5s;
}

.loader.hidden {
    opacity: 0;
    pointer-events: none;
}

.loader-spinner {
    width: 50px;
    height: 50px;
    border: 4px solid var(--color-gray-200);
    border-top-color: var(--color-primary);
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

@keyframes spin {
    to { transform: rotate(360deg); }
}
```

```javascript
window.addEventListener('load', () => {
    document.querySelector('.loader').classList.add('hidden');
});
```

---

## Icon Customization

### Using Custom SVG Icons

Replace existing SVG icons in HTML:

```html
<div class="service-icon">
    <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <!-- Your custom SVG path here -->
        <path d="..." fill="currentColor"/>
    </svg>
</div>
```

### Using Font Icons

1. Add Font Awesome (or similar):
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
```

2. Replace SVG with icon:
```html
<div class="service-icon">
    <i class="fas fa-robot"></i>
</div>
```

---

## Performance Optimization

### Image Optimization

```html
<!-- Use WebP with fallback -->
<picture>
    <source srcset="image.webp" type="image/webp">
    <img src="image.jpg" alt="Description">
</picture>
```

### Lazy Loading

```html
<img src="placeholder.jpg" data-src="actual-image.jpg" loading="lazy" alt="Description">
```

### CSS Optimization

```css
/* Use CSS containment for better performance */
.service-card {
    contain: layout style paint;
}
```

---

## Browser Compatibility

### Adding Vendor Prefixes

For older browsers, add prefixes:

```css
.element {
    -webkit-transform: translateY(0);
    -moz-transform: translateY(0);
    -ms-transform: translateY(0);
    transform: translateY(0);
}
```

### Polyfills for Older Browsers

Add to `index.html`:

```html
<script src="https://polyfill.io/v3/polyfill.min.js?features=IntersectionObserver"></script>
```

---

## Testing Your Customizations

1. **Visual Testing**: Check in multiple browsers (Chrome, Firefox, Safari, Edge)
2. **Mobile Testing**: Test on actual devices and Chrome DevTools mobile emulation
3. **Performance Testing**: Use Lighthouse in Chrome DevTools
4. **Accessibility Testing**: Use WAVE browser extension
5. **Cross-browser Testing**: Use BrowserStack or similar service

---

## Getting Help

- Check the main README.md for basic information
- Review the DEPLOYMENT.md for deployment-specific issues
- Search GitHub issues for similar problems
- Contact support at contact@gigspace.com

---

**Happy Customizing! 🎨**
