# Terra — Responsive Bento Grid Dashboard

## Project Overview
Terra is a modern financial dashboard component featuring a bento-style grid layout with strategic content placement and responsive design. Built with semantic HTML and advanced CSS Grid, it provides an elegant presentation for finance platform metrics and visual content.

## Live Preview

[View Live Demo](https://thisislefa.github.io/Terra)

## Technical Implementation

### Core Architecture
- **HTML5**: Semantic structure with proper heading hierarchy
- **CSS3**: CSS Grid with custom properties for theme management
- **No JavaScript**: Pure CSS implementation for optimal performance

### Grid System
The layout implements a sophisticated 3-column bento grid with asymmetric proportions:
```css
.bento-grid {
    display: grid;
    grid-template-columns: 1.6fr 1fr 1.2fr; /* Ratio-based column distribution */
    gap: 15px;
    position: relative;
}
```

### Design Token System
```css
:root {
    --font-family-base: 'Inter Tight', sans-serif;
    --color-bg: #ffffff;
    --color-text-main: #111111;
    --color-text-muted: #666666;
    --color-card-dark: #1a1a1a;
    --color-card-purple: #0070b5;
    --color-text-white: #ffffff;
    --color-accent-yellow: #eaff00;
    --border-radius-lg: 24px;
    --border-radius-sm: 12px;
}
```

## Responsive Design Strategy

### Breakpoint System
```css
/* Desktop: 3-column grid (default) */
/* Tablet: 1024px - 2-column grid */
@media (max-width: 1024px) {
    .bento-grid {
        grid-template-columns: 1fr 1fr;
    }
    .grid-col-right {
        grid-column: span 2;
    }
    .card-tall {
        height: 400px;
    }
}

/* Mobile: 768px - Single column stack */
@media (max-width: 768px) {
    .bento-grid {
        grid-template-columns: 1fr;
    }
    .grid-col-right {
        grid-column: span 1;
    }
}
```

## Integration Examples

### React Component
```jsx
import React from 'react';
import './Terra.css';

const TerraDashboard = ({ headline, subtext, cards }) => {
    return (
        <div className="layout-container">
            <header className="header-section">
                <div className="headline-wrapper">
                    <h1 className="main-headline">{headline}</h1>
                </div>
                <div className="subtext-wrapper">
                    <p className="header-subtext">{subtext}</p>
                </div>
            </header>
            
            <div className="bento-grid">
                {cards.map((card, index) => (
                    <div 
                        key={index} 
                        className={`card ${card.className}`}
                        style={{ gridArea: card.gridArea }}
                    >
                        {card.content}
                    </div>
                ))}
            </div>
        </div>
    );
};

export default TerraDashboard;
```

### Vue.js Implementation
```vue
<template>
    <div class="layout-container">
        <header class="header-section">
            <div class="headline-wrapper">
                <h1 class="main-headline">{{ headline }}</h1>
            </div>
            <div class="subtext-wrapper">
                <p class="header-subtext">{{ subtext }}</p>
            </div>
        </header>
        
        <div class="bento-grid">
            <div 
                v-for="(card, index) in cards"
                :key="index"
                :class="['card', card.className]"
                :style="{ gridArea: card.gridArea }"
            >
                <img 
                    v-if="card.imageUrl" 
                    :src="card.imageUrl" 
                    :alt="card.alt" 
                    class="card-img"
                />
                <div v-else class="card-content">
                    <div class="icon-bubble" v-if="card.icon">
                        <i :class="card.icon"></i>
                    </div>
                    <div class="stat-number">{{ card.stat }}</div>
                    <p class="card-description">{{ card.description }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        headline: String,
        subtext: String,
        cards: Array
    }
};
</script>
```

## Content Management System Integration

### Headless CMS Structure (Contentful)
```javascript
// Content model configuration
{
    "name": "Terra Dashboard",
    "fields": [
        {
            "id": "headline",
            "name": "Main Headline",
            "type": "Text",
            "required": true
        },
        {
            "id": "subtext",
            "name": "Header Subtext",
            "type": "Text",
            "required": true
        },
        {
            "id": "cards",
            "name": "Grid Cards",
            "type": "Array",
            "items": {
                "type": "Link",
                "linkType": "Entry"
            }
        }
    ]
}
```

### API Response Format
```json
{
    "headline": "Make payment easy, simplify your finance",
    "subtext": "Our platform managing personal finances...",
    "cards": [
        {
            "type": "image",
            "className": "card-consultation",
            "imageUrl": "https://example.com/image.jpg",
            "alt": "Consultation with doctor"
        },
        {
            "type": "stat",
            "className": "card-stat-purple",
            "icon": "fas fa-briefcase",
            "stat": "95%",
            "description": "Complete customer satisfaction..."
        }
    ]
}
```

## Performance Considerations

### Image Optimization
```html
<!-- Lazy loading with modern attributes -->
<img 
    src="image.jpg" 
    alt="Description" 
    loading="lazy" 
    decoding="async" 
    class="card-img"
/>
```

### CSS Delivery Optimization
```html
<!-- Critical CSS inlined for above-the-fold content -->
<style>
/* Minimal critical styles */
.layout-container { max-width: 1200px; margin: 0 auto; }
.header-section { display: flex; justify-content: space-between; }
.main-headline { font-size: 68px; line-height: 1.05; }
</style>

<!-- Async load full styles -->
<link rel="stylesheet" href="terra.css" media="print" onload="this.media='all'">
```

## Accessibility Features

### Implementation Details
- **Semantic HTML**: Proper use of `<header>`, `<section>`, and heading hierarchy
- **Color Contrast**: WCAG AA compliant contrast ratios maintained
- **Image Alt Text**: Descriptive alt attributes for all images
- **Focus Management**: Visible focus states for interactive elements
- **Screen Reader Support**: Logical content flow and ARIA labels

### Accessibility Testing
```javascript
// Example accessibility validation
function validateAccessibility() {
    // Check heading hierarchy
    const headings = document.querySelectorAll('h1, h2, h3, h4, h5, h6');
    // Verify proper heading order
    
    // Check color contrast
    // Verify minimum contrast ratios
    
    // Check image alt text
    const images = document.querySelectorAll('img');
    images.forEach(img => {
        if (!img.alt && !img.getAttribute('aria-hidden')) {
            console.warn('Image missing alt text:', img);
        }
    });
}
```

## Build and Deployment

### CSS Processing
```javascript
// PostCSS configuration
module.exports = {
    plugins: [
        require('postcss-preset-env')({
            stage: 3,
            features: {
                'nesting-rules': true,
                'custom-properties': true
            }
        }),
        require('cssnano')({
            preset: 'default'
        })
    ]
};
```

### Static Export Example
```javascript
// Next.js static generation
export async function getStaticProps() {
    const dashboardData = await fetchDashboardData();
    
    return {
        props: {
            dashboardData
        },
        revalidate: 3600 // Revalidate every hour
    };
}
```

## Browser Support

### Compatibility
- **Modern Browsers**: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **CSS Grid**: Full support in all modern browsers
- **CSS Custom Properties**: Full support
- **Fallbacks**: Graceful degradation for older browsers

### Feature Detection
```css
/* Progressive enhancement approach */
@supports (display: grid) {
    .bento-grid {
        display: grid;
    }
}

@supports not (display: grid) {
    .bento-grid {
        display: flex;
        flex-wrap: wrap;
    }
}
```

## Testing Strategy

### Visual Regression
```javascript
// Playwright visual tests
test.describe('Terra Dashboard Visual Tests', () => {
    test('should render correctly on desktop', async ({ page }) => {
        await page.goto('/dashboard');
        await expect(page.locator('.bento-grid')).toHaveScreenshot();
    });
    
    test('should be responsive on mobile', async ({ page }) => {
        await page.setViewportSize({ width: 375, height: 667 });
        await page.goto('/dashboard');
        await expect(page.locator('.bento-grid')).toHaveScreenshot();
    });
});
```

### Unit Tests
```javascript
// Component testing
describe('Terra Dashboard', () => {
    test('renders headline correctly', () => {
        const { getByText } = render(<TerraDashboard {...props} />);
        expect(getByText('Make payment easy')).toBeInTheDocument();
    });
    
    test('applies correct grid layout', () => {
        const { container } = render(<TerraDashboard {...props} />);
        expect(container.querySelector('.bento-grid')).toHaveStyle({
            display: 'grid'
        });
    });
});
```

## Maintenance and Updates

### Version Strategy
- **Semantic Versioning**: Follows semver for releases
- **Changelog**: Maintained in CHANGELOG.md
- **Deprecation Policy**: Clear deprecation warnings and migration paths

### Update Considerations
1. Monitor browser support for CSS features
2. Update dependencies regularly
3. Test across target devices and browsers
4. Validate accessibility compliance
5. Performance regression testing

## License and Usage
Terra is released under MIT License. Suitable for commercial applications requiring modern dashboard layouts with responsive design. The component is production-ready with comprehensive browser support.

For support or customization requests, contact through the project repository.

---

**Author**: [Lefa](https://github.com/thisislefa)  
**Technology**: Advanced CSS Grid with Design Tokens  
**Status**: Production Ready with Responsive Design
