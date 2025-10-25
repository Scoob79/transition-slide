🎬 Transition fluide entre fonds d’écran (effet flou + scroll)

Salut à tous 👋

Je voulais partager un petit script que j’ai conçu pour créer un effet de transition fluide entre plusieurs images de fond, entièrement en JavaScript natif (sans framework ni librairie).

Le principe est simple :
👉 au fur et à mesure du scroll, le fond d’écran change d’image.
L’image actuelle se floute et disparaît, pendant que la suivante devient nette et apparaît.
L’effet est bidirectionnel (il fonctionne aussi bien en descendant qu’en remontant la page).

🧠 Concept

Le code divise la page en trois plages de scroll :

de 0 à 30 % → Image 1

de 30 à 60 % → Image 2

de 60 à 100 % → Image 3

Chaque plage correspond à un fond affiché, avec un effet de fondu + flou progressif au passage d’une section à l’autre.

🧩 Structure HTML
<body>
  <div id="bg"></div>

  <main>
    <section data-bg="/images/bg1.jpg">Section 1</section>
    <section data-bg="/images/bg2.jpg">Section 2</section>
    <section data-bg="/images/bg3.jpg">Section 3</section>
  </main>
</body>

🎨 CSS
html, body {
  height: 300vh; /* 3 sections = 300 % de hauteur */
  margin: 0;
  overflow-x: hidden;
  font-family: sans-serif;
  color: #fff;
}

#bg {
  position: fixed;
  inset: 0;
  z-index: -1;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  opacity: 0;
  filter: blur(12px);
  transition: opacity 1s ease, filter 1s ease;
}

section {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
}

⚡ JavaScript
(() => {
  const bg = document.getElementById("bg");
  const sections = document.querySelectorAll("section[data-bg]");
  const images = Array.from(sections).map(s => s.dataset.bg);
  let currentIndex = -1;

  function changeBackground(idx) {
    if (idx === currentIndex) return;
    currentIndex = idx;

    // effet flou + fondu
    bg.style.filter = "blur(12px)";
    bg.style.opacity = "0";
    setTimeout(() => {
      bg.style.backgroundImage = `url('${images[idx]}')`;
      bg.style.opacity = "1";
      bg.style.filter = "blur(0px)";
    }, 300);
  }

  function update() {
    const doc = document.documentElement;
    const scrollTop = doc.scrollTop || window.pageYOffset;
    const max = doc.scrollHeight - doc.clientHeight;
    const p = max > 0 ? (scrollTop / max) * 100 : 0;
    const val = Math.round(Math.min(100, Math.max(0, p)));

    // 3 plages de scroll
    if (val < 30 && currentIndex !== 0) changeBackground(0);
    if (val >= 30 && val < 60 && currentIndex !== 1) changeBackground(1);
    if (val >= 60 && currentIndex !== 2) changeBackground(2);
  }

  window.addEventListener("scroll", () => requestAnimationFrame(update), { passive: true });
  window.addEventListener("load", update);
})();

✨ Résultat

🟢 Fluide : le fond se met à jour sans à-coups, avec un fondu net.
🟢 Réversible : l’effet fonctionne dans les deux sens du scroll.
🟢 Sans dépendances : 100 % vanilla JS, aucun framework requis.
🟢 Simple à personnaliser : ajoutez autant d’images que vous voulez, ajustez les plages de pourcentage selon vos besoins.


🌊 Smooth Background Slide Transition (Blur + Scroll)

Hey devs 👋
I’ve been experimenting with scroll-driven visual transitions for a website and came up with a lightweight vanilla JS approach for smooth background changes with a blur-to-focus effect — no libraries, no frameworks.

As you scroll, the background image fades and blurs out while the next one fades and sharpens in.
It’s fully bidirectional (works both scrolling up and down) and triggers based on scroll percentage.

🎬 Live demo concept

0–30 % → Image 1

30–60 % → Image 2

60–100 % → Image 3
Each range defines which image is active, with a gentle blur/fade transition between them.

🧩 HTML structure
<body>
  <div id="bg"></div>
  <main>
    <section data-bg="/images/bg1.jpg">Section 1</section>
    <section data-bg="/images/bg2.jpg">Section 2</section>
    <section data-bg="/images/bg3.jpg">Section 3</section>
  </main>
</body>

🎨 CSS
html, body {
  height: 300vh; /* enough to scroll */
  margin: 0;
  overflow-x: hidden;
  color: #fff;
  font-family: sans-serif;
}

#bg {
  position: fixed;
  inset: 0;
  z-index: -1;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  opacity: 0;
  filter: blur(12px);
  transition: opacity 1s ease, filter 1s ease;
}

section {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
}

⚡ JavaScript
(() => {
  const bg = document.getElementById("bg");
  const sections = document.querySelectorAll("section[data-bg]");
  const images = Array.from(sections).map(s => s.dataset.bg);
  let currentIndex = -1;

  function changeBackground(idx) {
    if (idx === currentIndex) return;
    currentIndex = idx;
    bg.style.filter = "blur(12px)";
    bg.style.opacity = "0";
    setTimeout(() => {
      bg.style.backgroundImage = `url('${images[idx]}')`;
      bg.style.opacity = "1";
      bg.style.filter = "blur(0px)";
    }, 300);
  }

  function update() {
    const doc = document.documentElement;
    const scrollTop = doc.scrollTop || window.pageYOffset;
    const max = doc.scrollHeight - doc.clientHeight;
    const p = max > 0 ? (scrollTop / max) * 100 : 0;
    const val = Math.round(Math.min(100, Math.max(0, p)));
    if (val < 30 && currentIndex !== 0) changeBackground(0);
    if (val >= 30 && val < 60 && currentIndex !== 1) changeBackground(1);
    if (val >= 60 && currentIndex !== 2) changeBackground(2);
  }

  window.addEventListener("scroll", () => requestAnimationFrame(update), { passive: true });
  window.addEventListener("load", update);
})();

✨ Features

✅ Pure vanilla JS (no dependencies)
✅ Works both ways — down & up
✅ Blur + fade transition for smoothness
✅ Light, reusable, and easily extendable
