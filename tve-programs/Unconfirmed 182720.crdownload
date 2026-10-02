(function () {
  'use strict';

  const slides = Array.from(document.querySelectorAll('.slide'));
  const total = slides.length;
  const progressBar = document.getElementById('progressBar');
  const counter = document.getElementById('counter');
  const prevBtn = document.getElementById('prevBtn');
  const nextBtn = document.getElementById('nextBtn');
  const fsBtn = document.getElementById('fsBtn');
  const restartBtn = document.getElementById('restartBtn');
  const slideList = document.getElementById('slideList');
  const menuEl = document.getElementById('slideMenu');

  let current = 0;

  // Build slide menu
  slides.forEach((slide, i) => {
    const item = document.createElement('button');
    item.type = 'button';
    item.className = 'list-group-item list-group-item-action';
    item.textContent = (i + 1) + '. ' + (slide.dataset.title || 'Slide ' + (i + 1));
    item.addEventListener('click', () => {
      goTo(i);
      const oc = bootstrap.Offcanvas.getInstance(menuEl);
      if (oc) oc.hide();
    });
    slideList.appendChild(item);
  });
  const menuItems = Array.from(slideList.children);

  function goTo(index) {
    current = Math.max(0, Math.min(total - 1, index));
    slides.forEach((s, i) => {
      s.classList.toggle('active', i === current);
      s.setAttribute('aria-hidden', i === current ? 'false' : 'true');
    });
    menuItems.forEach((m, i) => m.classList.toggle('active', i === current));
    slides[current].scrollTop = 0;

    counter.textContent = (current + 1) + ' / ' + total;
    progressBar.style.width = ((current + 1) / total * 100) + '%';
    prevBtn.disabled = current === 0;
    nextBtn.disabled = current === total - 1;

    if (history.replaceState) history.replaceState(null, '', '#' + (current + 1));
  }

  const next = () => goTo(current + 1);
  const prev = () => goTo(current - 1);

  function toggleFullscreen() {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen && document.documentElement.requestFullscreen();
    } else {
      document.exitFullscreen && document.exitFullscreen();
    }
  }

  // Buttons
  nextBtn.addEventListener('click', next);
  prevBtn.addEventListener('click', prev);
  fsBtn.addEventListener('click', toggleFullscreen);
  restartBtn.addEventListener('click', () => goTo(0));

  // Keyboard
  document.addEventListener('keydown', (e) => {
    if (e.target.closest && e.target.closest('.offcanvas.show')) return;
    switch (e.key) {
      case 'ArrowRight': case 'PageDown': case ' ': e.preventDefault(); next(); break;
      case 'ArrowLeft': case 'PageUp': e.preventDefault(); prev(); break;
      case 'Home': goTo(0); break;
      case 'End': goTo(total - 1); break;
      case 'f': case 'F': toggleFullscreen(); break;
    }
  });

  // Touch swipe (horizontal only, so vertical scrolling still works)
  let startX = 0, startY = 0;
  document.addEventListener('touchstart', (e) => {
    startX = e.changedTouches[0].clientX;
    startY = e.changedTouches[0].clientY;
  }, { passive: true });
  document.addEventListener('touchend', (e) => {
    const dx = e.changedTouches[0].clientX - startX;
    const dy = e.changedTouches[0].clientY - startY;
    if (Math.abs(dx) > 60 && Math.abs(dx) > Math.abs(dy) * 1.5) {
      dx < 0 ? next() : prev();
    }
  }, { passive: true });

  // Hash support (#5 opens slide 5)
  window.addEventListener('hashchange', () => {
    const n = parseInt(location.hash.slice(1), 10);
    if (!isNaN(n)) goTo(n - 1);
  });

  const start = parseInt(location.hash.slice(1), 10);
  goTo(!isNaN(start) ? start - 1 : 0);
})();
