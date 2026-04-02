---
description: Perform accessibility audit and provide WCAG compliance recommendations
agent: frontend-qa
temperature: 0.2
subtask: true
---

Perform comprehensive accessibility audit following WCAG 2.1 AA guidelines. Analyze code and provide remediation recommendations. When UI framework details matter, combine this with the frontend-development skill.

**Project Analysis:**
!`find . -type f \( -name "*.tsx" -o -name "*.jsx" -o -name "*.html" -o -name "*.vue" \) ! -path "./node_modules/*" ! -path "./dist/*" ! -path "./build/*" | head -20`

!`cat package.json 2>/dev/null | grep -E "(axe-core|pa11y|lighthouse)" || echo "Checking for accessibility testing tools..."`

!`grep -r "aria-" . --include="*.tsx" --include="*.jsx" --include="*.html" ! -path "./node_modules/*" ! -path "./dist/*" | wc -l`

## Accessibility Audit Report

### Summary
- **Files Analyzed**: [count]
- **WCAG Level**: [A/AA/AAA]
- **Overall Score**: [X/100]
- **Critical Issues**: [count]
- **Serious Issues**: [count]
- **Moderate Issues**: [count]
- **Minor Issues**: [count]

### WCAG Compliance Status
| Criterion | Level A | Level AA | Level AAA |
|-----------|---------|----------|-----------|
| Perceivable | [status] | [status] | [status] |
| Operable | [status] | [status] | [status] |
| Understandable | [status] | [status] | [status] |
| Robust | [status] | [status] | [status] |

---

## Critical Issues (Level A - Must Fix)

### 1. Missing Alt Text (WCAG 1.1.1)

**Severity**: Critical

**Files Affected**:
- [file:line] - `<img>` without alt attribute
- [file:line] - `<img>` with empty alt="" for decorative only

**Examples**:

```jsx
// Wrong (missing alt)
<img src="user-avatar.jpg" />

// Wrong (missing alt, decorative)
<img src="decoration.png" />

// Right (informative image)
<img 
  src="user-avatar.jpg" 
  alt="Profile photo of John Doe"
/>

// Right (decorative image)
<img 
  src="decoration.png" 
  alt=""
  role="presentation" 
/>

// Right (complex image)
<img 
  src="chart.png" 
  alt="Bar chart showing: Q1: 25%, Q2: 30%, Q3: 45%, Q4: 50%" 
/>
```

**Automated Fix**:
```jsx
// Add alt to all images without it
<img 
  src={src} 
  alt={alt || 'Image'} 
  {...props} 
/>
```

### 2. Missing Form Labels (WCAG 1.3.1, 4.1.2)

**Severity**: Critical

**Files Affected**:
- [file:line] - Input without label
- [file:line] - Label not associated with input

**Examples**:

```jsx
// Wrong (no label)
<input type="email" placeholder="Enter email" />
<input type="text" id="name" />

// Wrong (disconnected label)
<label>Name</label>
<input type="text" id="name" />

// Right (explicit association)
<label htmlFor="email">Email</label>
<input type="email" id="email" aria-describedby="emailHelp" />
<span id="emailHelp">We'll never share your email</span>

// Right (implicit association)
<label>
  Name
  <input type="text" />
</label>

// Right (aria-label)
<input 
  type="search" 
  aria-label="Search products" 
  placeholder="Search..." 
/>

// Right (aria-labelledby)
<div role="search">
  <h2 id="searchLabel">Search</h2>
  <input type="text" aria-labelledby="searchLabel" />
</div>
```

### 3. Missing Button Names (WCAG 2.4.4)

**Severity**: Critical

**Files Affected**:
- [file:line] - Button with no text/label
- [file:line] - Icon button without aria-label

**Examples**:

```jsx
// Wrong (empty button)
<button onClick={handleClick}></button>
<button onClick={handleClick}>
  <i className="icon-search"></i>
</button>

// Right (text button)
<button onClick={handleClick}>
  Search
</button>

// Right (icon button with aria-label)
<button 
  onClick={handleSearch}
  aria-label="Search products"
>
  <SearchIcon />
</button>

// Right (aria-label with text for screen readers)
<button 
  onClick={handleSubmit}
  aria-label="Submit form"
>
  <SubmitIcon />
  <span className="sr-only">Submit</span>
</button>
```

### 4. Missing Document Language (WCAG 3.1.1)

**Severity**: Critical

**Files Affected**:
- [file:line] - HTML without lang attribute
- [file:line] - Invalid lang attribute

**Examples**:

```html
<!-- Wrong (missing lang) -->
<html>
<head>...</head>
<body>...</body>
</html>

<!-- Wrong (invalid lang) -->
<html lang="en-US">
<head>...</head>
<body>...</body>
</html>

<!-- Right (valid lang) -->
<html lang="en">
<head>...</head>
<body>...</body>
</html>

<!-- Right (specific locale) -->
<html lang="en-GB">
<head>...</head>
<body>...</body>
</html>
```

### 5. Color Contrast Issues (WCAG 1.4.3)

**Severity**: Critical

**Files Affected**:
- [file:line] - Text with insufficient contrast
- [file:line] - Interactive elements with poor contrast

**Examples**:

```css
/* Wrong (low contrast - ratio 3.1:1) */
.text-muted {
  color: #999999;
  background-color: #ffffff;
}

/* Right (sufficient contrast - ratio 4.5:1) */
.text-muted {
  color: #666666; /* #666 is safe on white */
  background-color: #ffffff;
}

/* Right (large text - ratio 3:1) */
.heading-large {
  color: #777777; /* #777 is safe for 18pt+ */
  background-color: #ffffff;
}

/* Tools to use */
- contrast-checker.com
- Chrome DevTools > Lighthouse
- axe DevTools
```

---

## Serious Issues (Level AA - Should Fix)

### 1. Focus Management (WCAG 2.4.3, 2.4.7)

**Severity**: Serious

**Files Affected**:
- [file:line] - Focus not visible
- [file:line] - Focus order not logical
- [file:line] - Focus trapped in modal

**Examples**:

```jsx
// Wrong (removed outline)
*:focus {
  outline: none;
}

// Right (visible focus indicator)
*:focus-visible {
  outline: 2px solid #2563eb;
  outline-offset: 2px;
}

// Wrong (tab order issues)
<div tabIndex="0">First</div>
<div tabIndex="2">Third</div>
<div tabIndex="1">Second</div>

// Right (logical tab order)
<div tabIndex="0">First</div>
<div tabIndex="0">Second</div>
<div tabIndex="0">Third</div>

// Modal focus trap
function Modal({ isOpen, onClose, children }) {
  const modalRef = useRef(null);

  useEffect(() => {
    if (isOpen) {
      modalRef.current?.focus();
      document.body.style.overflow = 'hidden';
    } else {
      document.body.style.overflow = '';
    }
  }, [isOpen]);

  useEffect(() => {
    const handleTab = (e) => {
      if (e.key !== 'Tab') return;
      
      const focusable = modalRef.current?.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      );
      
      if (!focusable?.length) return;
      
      const first = focusable[0];
      const last = focusable[focusable.length - 1];
      
      if (e.shiftKey && document.activeElement === first) {
        e.preventDefault();
        last.focus();
      } else if (!e.shiftKey && document.activeElement === last) {
        e.preventDefault();
        first.focus();
      }
    };

    document.addEventListener('keydown', handleTab);
    return () => document.removeEventListener('keydown', handleTab);
  }, []);

  if (!isOpen) return null;

  return (
    <div 
      ref={modalRef}
      role="dialog"
      aria-modal="true"
      tabIndex={-1}
    >
      {children}
    </div>
  );
}
```

### 2. Skip Links (WCAG 2.4.1)

**Severity**: Serious

**Files Affected**:
- [file:line] - Missing skip link
- [file:line] - Skip link not visible

**Examples**:

```jsx
// Skip link component
function SkipLink() {
  return (
    <a 
      href="#main-content"
      className="skip-link"
    >
      Skip to main content
    </a>
  );
}

// CSS for skip link
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px 16px;
  z-index: 100;
  transition: top 0.3s;
}

.skip-link:focus {
  top: 0;
}

// Usage - place as first element in body
<body>
  <SkipLink />
  <header>...</header>
  <main id="main-content">
    {children}
  </main>
  <footer>...</footer>
</body>
```

### 3. ARIA Attributes (WCAG 4.1.2)

**Severity**: Serious

**Files Affected**:
- [file:line] - Invalid ARIA attribute
- [file:line] - Missing required ARIA
- [file:line] - ARIA attribute doesn't match role

**Examples**:

```jsx
// Wrong (invalid ARIA)
<div role="button" aria-pressed="maybe">Toggle</div>

// Right (valid ARIA)
<div 
  role="button" 
  aria-pressed={isPressed} 
  tabIndex={0}
  onClick={handleToggle}
>
  Toggle
</div>

// Wrong (missing required ARIA)
<slider min="0" max="100" value="50" />

// Right (with ARIA)
<input
  type="range"
  role="slider"
  aria-valuemin="0"
  aria-valuemax="100"
  aria-valuenow="50"
  aria-label="Volume"
/>

// Wrong (redundant ARIA)
<button role="button">Click me</button>

// Right (no redundant ARIA)
<button>Click me</button>
```

### 4. Error Identification (WCAG 3.3.1)

**Severity**: Serious

**Files Affected**:
- [file:line] - Form errors not identified
- [file:line] - Missing error messages

**Examples**:

```jsx
// Wrong (no error indication)
<input type="email" className={hasError ? 'error' : ''} />

// Right (with error identification)
<label htmlFor="email">Email</label>
<input
  type="email"
  id="email"
  aria-invalid={hasError}
  aria-describedby="emailError"
  aria-required="true"
/>
{hasError && (
  <span id="emailError" role="alert" className="error">
    Please enter a valid email address
  </span>
)}
```

### 5. Keyboard Navigation (WCAG 2.1.1)

**Severity**: Serious

**Files Affected**:
- [file:line] - Non-interactive element clickable
- [file:line] - Missing keyboard support

**Examples**:

```jsx
// Wrong (div instead of button)
<div onClick={handleClick}>Click me</div>

// Right (use button)
<button onClick={handleClick}>Click me</button>

// Wrong (custom component without keyboard)
<CustomDropdown items={options} onSelect={handleSelect} />

// Right (with keyboard support)
function CustomDropdown({ items, onSelect }) {
  const [isOpen, setIsOpen] = useState(false);
  const [focusedIndex, setFocusedIndex] = useState(-1);

  const handleKeyDown = (e) => {
    switch (e.key) {
      case 'Enter':
      case ' ':
        if (isOpen && focusedIndex >= 0) {
          e.preventDefault();
          onSelect(items[focusedIndex]);
        }
        setIsOpen(!isOpen);
        break;
      case 'Escape':
        setIsOpen(false);
        break;
      case 'ArrowDown':
        e.preventDefault();
        setFocusedIndex(i => Math.min(i + 1, items.length - 1));
        break;
      case 'ArrowUp':
        e.preventDefault();
        setFocusedIndex(i => Math.max(i - 1, 0));
        break;
      case 'Tab':
        setIsOpen(false);
        break;
    }
  };

  return (
    <div 
      role="combobox"
      aria-expanded={isOpen}
      aria-controls="dropdown-list"
      tabIndex={0}
      onKeyDown={handleKeyDown}
    >
      {/* Dropdown content */}
    </div>
  );
}
```

---

## Moderate Issues (Consider Fixing)

### 1. Heading Hierarchy (WCAG 1.3.1)

```jsx
// Wrong (skipped levels)
<h1>Main Title</h1>
<h3>Section Title</h3>

// Right (sequential)
<h1>Main Title</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>
```

### 2. Link Purpose (WCAG 2.4.4)

```jsx
// Wrong (unclear link text)
<a href="/products/123">Click here</a>
<a href="/products/123">Read more</a>

// Right (clear link text)
<a href="/products/123">View product details</a>
<a href="/products/123">Read more about Widget Pro</a>

// Right (with aria-label for icons)
<a href="/products/123" aria-label="View Widget Pro product page">
  <Icon />
</a>
```

### 3. List Structure (WCAG 1.3.1)

```jsx
// Wrong (not using list elements)
<div>- Item 1</div>
<div>- Item 2</div>

// Right (semantic lists)
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

---

## Testing Tools & Commands

### Automated Testing
```bash
# Install accessibility testing tools
npm install --save-dev axe-core @axe-core/react

# Run Lighthouse audit
npx lighthouse https://example.com --output=json

# Run pa11y
npx pa11y https://example.com

# Run axe in tests
npm install --save-dev @testing-library/axe
```

### Manual Testing Checklist
- [ ] Navigate using keyboard only
- [ ] Test with screen reader (NVDA, JAWS, VoiceOver)
- [ ] Verify color contrast
- [ ] Check focus indicators
- [ ] Test with zoom 200%
- [ ] Test reduced motion

---

## Compliance Roadmap

### Immediate (Week 1)
- [ ] Fix all Level A critical issues
- [ ] Add alt text to all images
- [ ] Label all form inputs
- [ ] Set document language
- [ ] Add skip link

### Short-term (Week 2)
- [ ] Fix all Level AA serious issues
- [ ] Implement focus management
- [ ] Add ARIA attributes
- [ ] Improve error handling
- [ ] Test keyboard navigation

### Medium-term (Month 1)
- [ ] Fix moderate issues
- [ ] Optimize heading hierarchy
- [ ] Improve link text
- [ ] Add more automated tests
- [ ] Document accessibility features

### Long-term (Quarter)
- [ ] Aim for Level AAA compliance
- [ ] Regular accessibility audits
- [ ] Accessibility training for team
- [ ] Continuous monitoring
- [ ] User testing with assistive technology

---

## Resources & References

### WCAG Guidelines
- [WCAG 2.1 Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/)
- [WCAG Techniques](https://www.w3.org/WAI/WCAG21/Techniques/)

### Tools
- [axe DevTools](https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [WAVE](https://wave.webaim.org/)
- [Contrast Checker](https://webaim.org/resources/contrastchecker/)

### Learning Resources
- [WebAIM](https://webaim.org/)
- [A11y Project](https://www.a11yproject.com/)
- [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
