# Interactive yet Simple Banking Website 🏦

Welcome to my engaging banking website! I have created this as my first static website using vanilla JavaScript. I applied elements such smooth scrolling, captivating animations, and handy modal windows for seamless navigation 💼💰.

## Table of Contents

- [Modal Window](#modal-window-)
- [Button Scrolling](#button-scrolling-)
- [Page Navigation](#page-navigation-)
- [Tabbed Component](#tabbed-component-)
- [Menu Fade Animation](#menu-fade-animation-)
- [Sticky Navigation](#sticky-navigation-)
- [Revealing Sections](#revealing-sections-)
- [Lazy Loading Images](#lazy-loading-images-)
- [Slider](#slider-)

## Modal Window 🖥️

The code implements a modal window functionality, which is a common UI pattern used to display important information or prompts to the user. The modal window is displayed on top of the main content and requires user interaction to be closed.

```javascript
const openModal = function (e) {
  e.preventDefault();
  modal.classList.remove('hidden');
  overlay.classList.remove('hidden');
};

const closeModal = function () {
  modal.classList.add('hidden');
  overlay.classList.add('hidden');
};
```

The `openModal` function removes the `hidden` class from the modal and overlay elements, making them visible. The `closeModal` function adds the `hidden` class back, effectively hiding the modal and overlay.

Event listeners are added to the buttons that open the modal, the close button, the overlay, and the `Escape` key to allow the user to close the modal.

## Button Scrolling 📜

This section implements smooth scrolling functionality when a button is clicked. It demonstrates the use of the `getBoundingClientRect()` method to retrieve the coordinates of an element relative to the viewport.

```javascript
btnScrollTo.addEventListener('click', function (e) {
  section1.scrollIntoView({ behavior: 'smooth' });
});
```

When the button is clicked, the `scrollIntoView` method is called on the target section, causing the page to smoothly scroll to that section.

## Page Navigation 🗺️

The code provides smooth scrolling functionality for the navigation links. When a navigation link is clicked, the corresponding section on the page is scrolled into view smoothly.

```javascript
document.querySelector('.nav__links').addEventListener('click', function (e) {
  e.preventDefault();

  if (e.target.classList.contains('nav__link')) {
    const id = e.target.getAttribute('href');
    document.querySelector(id).scrollIntoView({ behavior: 'smooth' });
  }
});
```

An event listener is added to the parent element of the navigation links. When a link is clicked, the code checks if the target element is a navigation link. If so, it retrieves the `href` attribute of the link, which contains the ID of the target section. The `scrollIntoView` method is then called on the target section, causing smooth scrolling.

## Tabbed Component 📂

The project includes a tabbed component, which allows users to switch between different content sections by clicking on tabs.

```javascript
tabsContainer.addEventListener('click', function (e) {
  const clicked = e.target.closest('.operations__tab');

  if (!clicked) return;

  tabs.forEach(t => t.classList.remove('operations__tab--active'));
  tabsContent.forEach(c => c.classList.remove('operations__content--active'));

  clicked.classList.add('operations__tab--active');
  document
    .querySelector(`.operations__content--${clicked.dataset.tab}`)
    .classList.add('operations__content--active');
});
```

An event listener is added to the container of the tabs. When a tab is clicked, the code finds the closest parent element with the `operations__tab` class. It then removes the `operations__tab--active` and `operations__content--active` classes from all tabs and content sections, respectively. Finally, it adds the active classes to the clicked tab and the corresponding content section.

## Menu Fade Animation 🖌️

This section implements a subtle animation effect on the navigation menu. When the user hovers over a navigation link, the other links and the logo fade out slightly.

```javascript
const handleHover = function (e) {
  if (e.target.classList.contains('nav__link')) {
    const link = e.target;
    const siblings = link.closest('.nav').querySelectorAll('.nav__link');
    const logo = link.closest('.nav').querySelector('img');

    siblings.forEach(el => {
      if (el !== link) el.style.opacity = this;
    });
    logo.style.opacity = this;
  }
};

nav.addEventListener('mouseover', handleHover.bind(0.5));
nav.addEventListener('mouseout', handleHover.bind(1));
```

The `handleHover` function is called when the user hovers over or leaves the navigation menu. It checks if the target element is a navigation link. If so, it retrieves the other links and the logo element. The opacity of the other links and the logo is then set to the value passed as an argument to the function (0.5 for hover, 1 for leaving).

The `bind` method is used to pass arguments to the `handleHover` function when it is called as an event listener.

## Sticky Navigation 📌

The code implements a sticky navigation bar that stays at the top of the viewport as the user scrolls down the page.

```javascript
const stickyNav = function (entries) {
  const [entry] = entries;

  if (!entry.isIntersecting) nav.classList.add('sticky');
  else nav.classList.remove('sticky');
};

const headerObserver = new IntersectionObserver(stickyNav, {
  root: null,
  threshold: 0,
  rootMargin: `-${navHeight}px`,
});

headerObserver.observe(header);
```

The `stickyNav` function is called by the `IntersectionObserver` whenever the observed element (the header) intersects or leaves the root element (the viewport). If the header is not intersecting with the viewport (i.e., it has scrolled out of view), the `sticky` class is added to the navigation bar. Otherwise, the `sticky` class is removed.

The `IntersectionObserver` is initialized with the `stickyNav` function as the callback and various options, including the root element, the threshold for triggering the callback, and the root margin (the space outside the root element that should be considered for intersection).

## Revealing Sections 🔍

This section implements a feature where content sections are initially hidden and revealed as the user scrolls down the page.

```javascript
const revealSection = function (entries, observer) {
  const [entry] = entries;

  if (!entry.isIntersecting) return;

  entry.target.classList.remove('section--hidden');
  observer.unobserve(entry.target);
};

const sectionObserver = new IntersectionObserver(revealSection, {
  root: null,
  threshold: 0.15,
});

allSections.forEach(function (section) {
  sectionObserver.observe(section);
  section.classList.add('section--hidden');
});
```

The `revealSection` function is called by the `IntersectionObserver` whenever an observed section intersects with the root element (the viewport).

 If the section is intersecting, the `section--hidden` class is removed from the section, revealing its content. The observer is then instructed to stop observing that section.

The `IntersectionObserver` is initialized with the `revealSection` function as the callback and various options, including the root element and the threshold for triggering the callback.

All sections are initially assigned the `section--hidden` class to hide their content. The `IntersectionObserver` is then instructed to observe each section.

## Lazy Loading Images ⚡

The project includes a lazy loading feature for images, which improves performance by loading images only when they are about to become visible in the viewport.

```javascript
const loadImg = function (entries, observer) {
  const [entry] = entries;

  if (!entry.isIntersecting) return;

  entry.target.src = entry.target.dataset.src;

  entry.target.addEventListener('load', function () {
    entry.target.classList.remove('lazy-img');
  });

  observer.unobserve(entry.target);
};

const imgObserver = new IntersectionObserver(loadImg, {
  root: null,
  threshold: 0,
  rootMargin: '200px',
});

imgTargets.forEach(img => imgObserver.observe(img));
```

The `loadImg` function is called by the `IntersectionObserver` whenever an observed image is about to become visible in the viewport. If the image is intersecting, its `src` attribute is set to the value of the `data-src` attribute, effectively loading the image. Once the image has finished loading, the `lazy-img` class is removed from the image element.

The `IntersectionObserver` is initialized with the `loadImg` function as the callback and various options, including the root element, the threshold for triggering the callback, and the root margin (the space outside the root element that should be considered for intersection).

All images with the `data-src` attribute are selected, and the `IntersectionObserver` is instructed to observe each of them.

## Slider 🎢

The project includes a slider component that allows users to navigate through a series of slides or images.

```javascript
const slider = function () {
  // ... (implementation details omitted for brevity)

  const nextSlide = function () {
    if (curSlide === maxSlide - 1) {
      curSlide = 0;
    } else {
      curSlide++;
    }

    goToSlide(curSlide);
    activateDot(curSlide);
  };

  const prevSlide = function () {
    if (curSlide === 0) {
      curSlide = maxSlide - 1;
    } else {
      curSlide--;
    }
    goToSlide(curSlide);
    activateDot(curSlide);
  };

  // Event handlers
  btnRight.addEventListener('click', nextSlide);
  btnLeft.addEventListener('click', prevSlide);

  document.addEventListener('keydown', function (e) {
    if (e.key === 'ArrowLeft') prevSlide();
    e.key === 'ArrowRight' && nextSlide();
  });

  // ... (implementation details omitted for brevity)
};
slider();
```
The `slider` function encapsulates the logic for the slider component. It includes functions for navigating to the next and previous slides (`nextSlide` and `prevSlide`), as well as functions for creating and updating the navigation dots.

Event listeners are added to the left and right buttons, as well as the keyboard arrow keys, to allow users to navigate through the slides.

The `slider` function is called at the end of the script to initialize the slider component.
```


Feel free to customize further as needed for your project!

Happy coding :) 

