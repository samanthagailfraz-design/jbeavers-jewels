# jbeavers-jewels
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>J Beaver's Jewels — Heirloom Fine Jewelry</title>
<meta name="description" content="J Beaver's Jewels designs and crafts heirloom fine jewelry in the United States. Ethically sourced, hand finished, certified for authenticity." />
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,400;0,9..144,500;0,9..144,600;1,9..144,300;1,9..144,400&family=Manrope:wght@300;400;500;600&display=swap" rel="stylesheet" />
<script src="https://js.stripe.com/v3/"></script>
<script src="config.js"></script>
<style>
  :root {
    --cream: #f5ede1;
    --cream-deep: #ede2cf;
    --paper: #fbf6ec;
    --ink: #1f140d;
    --ink-soft: #4a3a2c;
    --muted: #8a7560;
    --burgundy: #6b1f2a;
    --burgundy-dark: #4a151d;
    --gold: #b8884c;
    --gold-bright: #d4a464;
    --line: rgba(31, 20, 13, 0.12);
    --line-strong: rgba(31, 20, 13, 0.25);
    --display: "Fraunces", "Times New Roman", serif;
    --body: "Manrope", -apple-system, sans-serif;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }
  body {
    background: var(--cream);
    color: var(--ink);
    font-family: var(--body);
    font-weight: 300;
    line-height: 1.6;
    overflow-x: hidden;
  }
  body::before {
    content: "";
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 999;
    opacity: 0.05;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  }
  ::selection { background: var(--burgundy); color: var(--cream); }

  /* ========== NAVIGATION ========== */
  nav {
    position: fixed; top: 0; left: 0; right: 0;
    padding: 1.2rem 2.5rem;
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    align-items: center;
    z-index: 100;
    background: rgba(245, 237, 225, 0.92);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--line);
  }
  .nav-left { display: flex; gap: 2rem; }
  .nav-right { display: flex; gap: 1.5rem; justify-content: flex-end; align-items: center; }
  .nav-link {
    color: var(--ink);
    text-decoration: none;
    font-size: 0.74rem;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    font-weight: 500;
    transition: color 0.3s;
    position: relative;
  }
  .nav-link::after {
    content: "";
    position: absolute;
    left: 0; right: 0; bottom: -4px;
    height: 1px;
    background: var(--burgundy);
    transform: scaleX(0);
    transition: transform 0.4s;
  }
  .nav-link:hover { color: var(--burgundy); }
  .nav-link:hover::after { transform: scaleX(1); }
  .logo {
    font-family: var(--display);
    font-size: 1.4rem;
    font-weight: 400;
    letter-spacing: 0.02em;
    color: var(--ink);
    text-align: center;
    white-space: nowrap;
  }
  .logo em { color: var(--burgundy); font-style: italic; font-weight: 500; }
  .cart-trigger {
    background: none;
    border: 1px solid var(--ink);
    color: var(--ink);
    padding: 0.55rem 1.1rem;
    font-family: var(--body);
    font-size: 0.7rem;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  .cart-trigger:hover { background: var(--ink); color: var(--cream); }
  .cart-count {
    background: var(--burgundy);
    color: var(--cream);
    border-radius: 50%;
    width: 18px; height: 18px;
    font-size: 0.6rem;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.3s;
  }
  .cart-count.bump { transform: scale(1.4); }
  .mobile-toggle { display: none; background: none; border: none; cursor: pointer; }
  .mobile-toggle span {
    display: block; width: 22px; height: 1.5px;
    background: var(--ink); margin: 5px 0;
    transition: 0.3s;
  }

  @media (max-width: 900px) {
    nav { grid-template-columns: auto 1fr auto; padding: 1rem 1.25rem; }
    .nav-left { display: none; }
    .nav-right .nav-link { display: none; }
    .mobile-toggle { display: block; }
    .logo { text-align: left; font-size: 1.15rem; }
  }

  .mobile-menu {
    position: fixed; top: 0; left: 0; right: 0; bottom: 0;
    background: var(--cream);
    z-index: 95;
    padding: 6rem 2rem 2rem;
    transform: translateY(-100%);
    transition: transform 0.5s cubic-bezier(0.65,0,0.35,1);
  }
  .mobile-menu.open { transform: translateY(0); }
  .mobile-menu a {
    display: block;
    font-family: var(--display);
    font-size: 2rem;
    color: var(--ink);
    text-decoration: none;
    padding: 1rem 0;
    border-bottom: 1px solid var(--line);
    font-weight: 300;
  }
  .mobile-menu a em { font-style: italic; color: var(--burgundy); }

  /* ========== HERO ========== */
  .hero {
    min-height: 100vh;
    padding: 8rem 2.5rem 4rem;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
    position: relative;
    overflow: hidden;
  }
  .hero::before {
    content: "J";
    position: absolute;
    top: -10%; right: -8%;
    font-family: var(--display);
    font-size: 70vw;
    font-style: italic;
    font-weight: 300;
    color: var(--burgundy);
    opacity: 0.04;
    line-height: 0.8;
    pointer-events: none;
  }
  .hero-text { position: relative; z-index: 2; }
  .hero-eyebrow {
    font-size: 0.72rem;
    letter-spacing: 0.4em;
    text-transform: uppercase;
    color: var(--burgundy);
    margin-bottom: 2rem;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 1rem;
    opacity: 0;
    animation: fadeRight 1s 0.2s forwards;
  }
  .hero-eyebrow::before {
    content: "";
    width: 40px; height: 1px;
    background: var(--burgundy);
  }
  .hero h1 {
    font-family: var(--display);
    font-size: clamp(3rem, 7vw, 6.5rem);
    font-weight: 300;
    line-height: 0.95;
    letter-spacing: -0.02em;
    margin-bottom: 2rem;
    opacity: 0;
    animation: fadeUp 1.2s 0.4s forwards;
  }
  .hero h1 em {
    font-style: italic;
    color: var(--burgundy);
    font-weight: 400;
  }
  .hero-sub {
    font-size: 1.05rem;
    color: var(--ink-soft);
    max-width: 440px;
    margin-bottom: 2.5rem;
    line-height: 1.7;
    opacity: 0;
    animation: fadeUp 1.4s 0.6s forwards;
  }
  .hero-actions {
    display: flex; gap: 1rem; flex-wrap: wrap;
    opacity: 0;
    animation: fadeUp 1.6s 0.8s forwards;
  }
  .btn {
    display: inline-block;
    padding: 1rem 2.2rem;
    text-decoration: none;
    font-family: var(--body);
    font-size: 0.74rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    font-weight: 500;
    border: 1px solid;
    cursor: pointer;
    transition: all 0.4s;
    background: none;
  }
  .btn-primary {
    background: var(--ink);
    color: var(--cream);
    border-color: var(--ink);
  }
  .btn-primary:hover {
    background: var(--burgundy);
    border-color: var(--burgundy);
    transform: translateY(-2px);
  }
  .btn-secondary {
    background: none;
    color: var(--ink);
    border-color: var(--ink);
  }
  .btn-secondary:hover {
    background: var(--ink);
    color: var(--cream);
  }

  .hero-visual {
    position: relative;
    aspect-ratio: 4/5;
    background: linear-gradient(135deg, var(--burgundy), var(--burgundy-dark));
    display: flex; align-items: center; justify-content: center;
    overflow: hidden;
    opacity: 0;
    animation: fadeIn 1.5s 0.5s forwards;
  }
  .hero-est {
    position: absolute;
    bottom: 1.5rem; right: 1.5rem;
    font-size: 0.65rem;
    letter-spacing: 0.4em;
    color: var(--gold);
    writing-mode: vertical-rl;
    transform: rotate(180deg);
  }
  .hero-visual svg {
    width: 65%; height: 65%;
    filter: drop-shadow(0 20px 40px rgba(0,0,0,0.4));
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeRight {
    from { opacity: 0; transform: translateX(-20px); }
    to { opacity: 1; transform: translateX(0); }
  }
  @keyframes fadeIn { to { opacity: 1; } }

  @media (max-width: 900px) {
    .hero {
      grid-template-columns: 1fr;
      gap: 3rem;
      padding: 6rem 1.5rem 3rem;
    }
    .hero-visual { aspect-ratio: 1; max-width: 380px; margin: 0 auto; }
  }

  /* ========== MARQUEE ========== */
  .marquee {
    background: var(--ink);
    color: var(--cream);
    padding: 1rem 0;
    overflow: hidden;
    border-top: 1px solid var(--ink);
    border-bottom: 1px solid var(--ink);
  }
  .marquee-track {
    display: flex;
    gap: 3rem;
    animation: scroll 35s linear infinite;
    white-space: nowrap;
    width: max-content;
  }
  .marquee span {
    font-family: var(--display);
    font-style: italic;
    font-size: 1.2rem;
    color: var(--cream);
    display: flex;
    align-items: center;
    gap: 3rem;
  }
  .marquee span::after {
    content: "✦";
    color: var(--gold);
    font-style: normal;
  }
  @keyframes scroll {
    to { transform: translateX(-50%); }
  }

  /* ========== SECTION COMMONS ========== */
  section { padding: 7rem 2.5rem; }
  .container { max-width: 1300px; margin: 0 auto; }
  .section-header {
    text-align: center;
    margin-bottom: 4.5rem;
    position: relative;
  }
  .section-header.left { text-align: left; max-width: 1300px; margin-left: auto; margin-right: auto; margin-bottom: 4.5rem; }
  .eyebrow {
    font-size: 0.7rem;
    letter-spacing: 0.4em;
    text-transform: uppercase;
    color: var(--burgundy);
    margin-bottom: 1rem;
    font-weight: 500;
  }
  .section-title {
    font-family: var(--display);
    font-size: clamp(2.2rem, 5vw, 4rem);
    font-weight: 300;
    line-height: 1.05;
    letter-spacing: -0.01em;
  }
  .section-title em { font-style: italic; color: var(--burgundy); }
  .section-desc {
    font-size: 1rem;
    color: var(--ink-soft);
    max-width: 540px;
    margin: 1.5rem auto 0;
    line-height: 1.8;
  }
  .section-header.left .section-desc { margin-left: 0; }

  @media (max-width: 768px) {
    section { padding: 5rem 1.25rem; }
  }

  /* ========== FEATURED TRIO ========== */
  .featured {
    background: var(--paper);
    border-top: 1px solid var(--line);
    border-bottom: 1px solid var(--line);
  }
  .featured-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }
  .featured-card {
    text-align: center;
    cursor: pointer;
  }
  .featured-image {
    aspect-ratio: 4/5;
    background: var(--cream-deep);
    margin-bottom: 1.5rem;
    display: flex; align-items: center; justify-content: center;
    position: relative;
    overflow: hidden;
    transition: transform 0.6s;
  }
  .featured-image svg {
    width: 55%; height: 55%;
    transition: transform 0.8s cubic-bezier(0.4,0,0.2,1);
  }
  .featured-card:hover .featured-image svg { transform: scale(1.1) rotate(3deg); }
  .featured-card:hover .featured-image { background: #e2d4ba; }
  .featured-name {
    font-family: var(--display);
    font-size: 1.5rem;
    font-weight: 400;
    margin-bottom: 0.4rem;
  }
  .featured-price {
    font-family: var(--display);
    font-style: italic;
    color: var(--burgundy);
    font-size: 1.1rem;
  }
  @media (max-width: 768px) {
    .featured-grid { grid-template-columns: 1fr; gap: 3rem; }
  }

  /* ========== COLLECTION ========== */
  .filters {
    display: flex;
    gap: 0.5rem;
    justify-content: center;
    margin-bottom: 3.5rem;
    flex-wrap: wrap;
  }
  .filter-btn {
    background: none;
    border: 1px solid var(--line-strong);
    color: var(--ink);
    padding: 0.6rem 1.4rem;
    font-family: var(--body);
    font-size: 0.7rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.3s;
  }
  .filter-btn:hover { border-color: var(--ink); }
  .filter-btn.active {
    background: var(--ink);
    color: var(--cream);
    border-color: var(--ink);
  }

  .product-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(270px, 1fr));
    gap: 2.5rem 2rem;
  }
  .product {
    background: var(--paper);
    border: 1px solid transparent;
    transition: all 0.5s;
    cursor: pointer;
    display: flex;
    flex-direction: column;
  }
  .product:hover {
    border-color: var(--line-strong);
    transform: translateY(-4px);
  }
  .product-image {
    aspect-ratio: 1;
    background: var(--cream-deep);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  .product-image svg {
    width: 60%; height: 60%;
    transition: transform 0.7s;
  }
  .product:hover .product-image svg { transform: scale(1.08); }
  .product-badge {
    position: absolute;
    top: 1rem; left: 1rem;
    background: var(--burgundy);
    color: var(--cream);
    font-size: 0.6rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    padding: 0.35rem 0.8rem;
    font-weight: 500;
  }
  .product-info {
    padding: 1.5rem 1.4rem 1.6rem;
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  .product-cat {
    font-size: 0.65rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 0.5rem;
  }
  .product-name {
    font-family: var(--display);
    font-size: 1.35rem;
    font-weight: 400;
    margin-bottom: 0.4rem;
    line-height: 1.2;
  }
  .product-desc {
    font-size: 0.85rem;
    color: var(--ink-soft);
    margin-bottom: 1.2rem;
    flex: 1;
    line-height: 1.6;
  }
  .product-bottom {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-top: 1rem;
    border-top: 1px solid var(--line);
  }
  .product-price {
    font-family: var(--display);
    font-size: 1.3rem;
    font-style: italic;
    color: var(--burgundy);
  }
  .add-btn {
    background: var(--ink);
    border: 1px solid var(--ink);
    color: var(--cream);
    padding: 0.55rem 1.1rem;
    font-family: var(--body);
    font-size: 0.65rem;
    letter-spacing: 0.22em;
    text-transform: uppercase;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s;
  }
  .add-btn:hover {
    background: var(--burgundy);
    border-color: var(--burgundy);
  }
  .add-btn.added {
    background: var(--gold);
    border-color: var(--gold);
    color: var(--ink);
  }

  /* ========== EDITORIAL SPLIT ========== */
  .split {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0;
    min-height: 600px;
    border-top: 1px solid var(--line);
  }
  .split-visual {
    background: linear-gradient(135deg, var(--burgundy-dark), var(--burgundy));
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  .split-visual svg { width: 50%; max-width: 320px; }
  .split-visual::before {
    content: "&";
    position: absolute;
    font-family: var(--display);
    font-style: italic;
    font-size: 30vw;
    color: var(--gold);
    opacity: 0.08;
    line-height: 1;
    bottom: -5%; left: -5%;
  }
  .split-text {
    padding: 5rem 4rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  .split-text h2 {
    font-family: var(--display);
    font-size: clamp(2rem, 4vw, 3.2rem);
    font-weight: 300;
    line-height: 1.1;
    margin-bottom: 2rem;
  }
  .split-text h2 em { font-style: italic; color: var(--burgundy); }
  .split-text p {
    color: var(--ink-soft);
    font-size: 1rem;
    line-height: 1.8;
    margin-bottom: 1.2rem;
  }
  .signature {
    font-family: var(--display);
    font-style: italic;
    font-size: 1.6rem;
    color: var(--burgundy);
    margin-top: 2rem;
  }
  @media (max-width: 900px) {
    .split { grid-template-columns: 1fr; }
    .split-visual { aspect-ratio: 1; min-height: auto; }
    .split-text { padding: 4rem 2rem; }
  }

  /* ========== CRAFT PROCESS ========== */
  .craft {
    background: var(--paper);
    border-top: 1px solid var(--line);
    border-bottom: 1px solid var(--line);
  }
  .craft-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2.5rem;
    margin-top: 2rem;
  }
  .craft-step {
    text-align: center;
    padding: 2rem 1rem;
    border-top: 1px solid var(--line-strong);
    position: relative;
  }
  .craft-num {
    font-family: var(--display);
    font-style: italic;
    font-size: 3rem;
    color: var(--burgundy);
    font-weight: 300;
    margin-bottom: 1rem;
    display: block;
  }
  .craft-step h3 {
    font-family: var(--display);
    font-size: 1.4rem;
    font-weight: 400;
    margin-bottom: 1rem;
  }
  .craft-step p {
    font-size: 0.88rem;
    color: var(--ink-soft);
    line-height: 1.7;
  }
  @media (max-width: 768px) {
    .craft-grid { grid-template-columns: 1fr 1fr; gap: 1.5rem; }
  }
  @media (max-width: 480px) {
    .craft-grid { grid-template-columns: 1fr; }
  }

  /* ========== TESTIMONIALS ========== */
  .testimonial-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2.5rem;
  }
  .testimonial {
    padding: 2.5rem;
    background: var(--paper);
    border: 1px solid var(--line);
    position: relative;
  }
  .testimonial::before {
    content: '"';
    position: absolute;
    top: -1rem; left: 1.5rem;
    font-family: var(--display);
    font-style: italic;
    font-size: 5rem;
    color: var(--burgundy);
    line-height: 1;
  }
  .testimonial-text {
    font-family: var(--display);
    font-style: italic;
    font-size: 1.1rem;
    line-height: 1.6;
    color: var(--ink);
    margin-bottom: 1.5rem;
    margin-top: 0.5rem;
  }
  .testimonial-author {
    font-size: 0.72rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--burgundy);
    font-weight: 500;
  }
  .stars {
    color: var(--gold);
    letter-spacing: 0.1em;
    margin-bottom: 1rem;
    font-size: 0.9rem;
  }
  @media (max-width: 900px) {
    .testimonial-grid { grid-template-columns: 1fr; }
  }

  /* ========== BESPOKE BANNER ========== */
  .bespoke {
    background: var(--ink);
    color: var(--cream);
    text-align: center;
    padding: 7rem 2rem;
    position: relative;
    overflow: hidden;
  }
  .bespoke::before, .bespoke::after {
    content: "✦";
    position: absolute;
    color: var(--gold);
    font-size: 1.5rem;
    opacity: 0.4;
  }
  .bespoke::before { top: 2rem; left: 50%; transform: translateX(-50%); }
  .bespoke::after { bottom: 2rem; left: 50%; transform: translateX(-50%); }
  .bespoke .eyebrow { color: var(--gold); }
  .bespoke h2 {
    font-family: var(--display);
    font-size: clamp(2.2rem, 5vw, 4rem);
    font-weight: 300;
    line-height: 1.1;
    margin: 1rem auto 2rem;
    max-width: 800px;
  }
  .bespoke h2 em { color: var(--gold); font-style: italic; }
  .bespoke p {
    color: var(--cream);
    opacity: 0.75;
    max-width: 540px;
    margin: 0 auto 2.5rem;
    font-size: 1rem;
  }
  .btn-gold {
    background: var(--gold);
    border-color: var(--gold);
    color: var(--ink);
  }
  .btn-gold:hover {
    background: var(--gold-bright);
    border-color: var(--gold-bright);
  }

  /* ========== FAQ ========== */
  .faq-list { max-width: 800px; margin: 0 auto; }
  .faq-item {
    border-bottom: 1px solid var(--line-strong);
  }
  .faq-q {
    width: 100%;
    background: none;
    border: none;
    padding: 1.6rem 0;
    text-align: left;
    cursor: pointer;
    font-family: var(--display);
    font-size: 1.25rem;
    color: var(--ink);
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: 400;
    transition: color 0.3s;
  }
  .faq-q:hover { color: var(--burgundy); }
  .faq-q::after {
    content: "+";
    font-size: 1.5rem;
    font-weight: 300;
    transition: transform 0.4s;
    color: var(--burgundy);
  }
  .faq-item.open .faq-q::after { transform: rotate(45deg); }
  .faq-a {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.5s ease, padding 0.5s ease;
    color: var(--ink-soft);
    font-size: 0.95rem;
    line-height: 1.8;
  }
  .faq-item.open .faq-a {
    max-height: 400px;
    padding-bottom: 1.6rem;
  }

  /* ========== NEWSLETTER ========== */
  .newsletter {
    background: var(--cream-deep);
    text-align: center;
    border-top: 1px solid var(--line);
  }
  .newsletter-form {
    display: flex;
    max-width: 480px;
    margin: 2rem auto 0;
    border-bottom: 1px solid var(--ink);
  }
  .newsletter-form input {
    flex: 1;
    background: none;
    border: none;
    padding: 1rem 0;
    font-family: var(--body);
    font-size: 0.95rem;
    color: var(--ink);
    outline: none;
  }
  .newsletter-form input::placeholder { color: var(--muted); }
  .newsletter-form button {
    background: none;
    border: none;
    color: var(--ink);
    padding: 1rem;
    cursor: pointer;
    font-family: var(--body);
    font-size: 0.72rem;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    font-weight: 500;
    transition: color 0.3s;
  }
  .newsletter-form button:hover { color: var(--burgundy); }
  .newsletter-success {
    margin-top: 1.5rem;
    color: var(--burgundy);
    font-family: var(--display);
    font-style: italic;
    font-size: 1.1rem;
    opacity: 0;
    transition: opacity 0.5s;
  }
  .newsletter-success.show { opacity: 1; }

  /* ========== CONTACT ========== */
  .contact { background: var(--paper); border-top: 1px solid var(--line); }
  .contact-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 3rem;
    margin-top: 1rem;
  }
  .contact-card {
    text-align: center;
    padding: 2rem 1rem;
  }
  .contact-card h5 {
    font-size: 0.7rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--burgundy);
    margin-bottom: 1rem;
    font-weight: 500;
  }
  .contact-card .icon {
    font-family: var(--display);
    font-size: 2rem;
    font-style: italic;
    color: var(--gold);
    margin-bottom: 1rem;
  }
  .contact-card p {
    font-family: var(--display);
    font-size: 1.2rem;
    color: var(--ink);
    line-height: 1.5;
  }
  .contact-card a {
    color: var(--ink);
    text-decoration: none;
    border-bottom: 1px solid var(--line-strong);
    transition: border-color 0.3s;
    word-break: break-word;
  }
  .contact-card a:hover { border-color: var(--burgundy); color: var(--burgundy); }
  @media (max-width: 768px) {
    .contact-grid { grid-template-columns: 1fr; gap: 1rem; }
  }

  /* ========== FOOTER ========== */
  footer {
    background: var(--ink);
    color: var(--cream);
    padding: 4rem 2.5rem 2rem;
  }
  .footer-grid {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr 1fr;
    gap: 3rem;
    max-width: 1300px;
    margin: 0 auto 3rem;
  }
  .footer-brand .logo { color: var(--cream); text-align: left; font-size: 1.6rem; }
  .footer-brand p {
    font-size: 0.85rem;
    color: rgba(245, 237, 225, 0.6);
    margin-top: 1rem;
    line-height: 1.7;
    max-width: 300px;
  }
  .footer-col h6 {
    font-size: 0.7rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 1.2rem;
    font-weight: 500;
  }
  .footer-col a {
    display: block;
    color: rgba(245, 237, 225, 0.7);
    text-decoration: none;
    padding: 0.35rem 0;
    font-size: 0.88rem;
    transition: color 0.3s;
  }
  .footer-col a:hover { color: var(--gold); }
  .footer-bottom {
    border-top: 1px solid rgba(245, 237, 225, 0.1);
    padding-top: 2rem;
    text-align: center;
    font-size: 0.78rem;
    color: rgba(245, 237, 225, 0.5);
    max-width: 1300px;
    margin: 0 auto;
  }
  .footer-bottom .stripe-mark {
    color: var(--gold);
    letter-spacing: 0.15em;
  }
  @media (max-width: 768px) {
    .footer-grid { grid-template-columns: 1fr 1fr; gap: 2rem; }
    .footer-brand { grid-column: 1 / -1; }
  }

  /* ========== CART DRAWER ========== */
  .overlay {
    position: fixed;
    inset: 0;
    background: rgba(31, 20, 13, 0.5);
    backdrop-filter: blur(6px);
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.4s;
    z-index: 200;
  }
  .overlay.open { opacity: 1; pointer-events: all; }
  .cart-drawer {
    position: fixed;
    top: 0; right: 0;
    width: 100%;
    max-width: 460px;
    height: 100vh;
    background: var(--cream);
    transform: translateX(100%);
    transition: transform 0.5s cubic-bezier(0.65,0,0.35,1);
    z-index: 201;
    display: flex;
    flex-direction: column;
    border-left: 1px solid var(--line);
  }
  .cart-drawer.open { transform: translateX(0); }
  .cart-header {
    padding: 1.5rem;
    border-bottom: 1px solid var(--line);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .cart-header h3 {
    font-family: var(--display);
    font-size: 1.7rem;
    font-weight: 400;
  }
  .icon-btn {
    background: none;
    border: none;
    color: var(--ink);
    font-size: 1.5rem;
    cursor: pointer;
    padding: 0.5rem;
    line-height: 1;
    transition: color 0.3s;
  }
  .icon-btn:hover { color: var(--burgundy); }
  .cart-items { flex: 1; overflow-y: auto; padding: 1.5rem; }
  .cart-item {
    display: flex;
    gap: 1rem;
    padding: 1.2rem 0;
    border-bottom: 1px solid var(--line);
  }
  .cart-item-img {
    width: 84px; height: 84px;
    background: var(--cream-deep);
    flex-shrink: 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .cart-item-img svg { width: 70%; height: 70%; }
  .cart-item-info { flex: 1; display: flex; flex-direction: column; }
  .cart-item-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 0.5rem;
    margin-bottom: 0.4rem;
  }
  .cart-item-name { font-family: var(--display); font-size: 1.05rem; font-weight: 500; }
  .cart-item-remove {
    background: none; border: none;
    color: var(--muted);
    cursor: pointer;
    font-size: 0.7rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    transition: color 0.3s;
  }
  .cart-item-remove:hover { color: var(--burgundy); }
  .cart-item-price {
    font-family: var(--display);
    font-style: italic;
    color: var(--burgundy);
    font-size: 0.95rem;
    margin-bottom: 0.6rem;
  }
  .qty {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    border: 1px solid var(--line-strong);
    width: fit-content;
  }
  .qty button {
    background: none;
    border: none;
    color: var(--ink);
    width: 28px; height: 28px;
    cursor: pointer;
    transition: background 0.3s;
  }
  .qty button:hover { background: var(--cream-deep); }
  .qty span { min-width: 24px; text-align: center; font-size: 0.9rem; }
  .cart-empty {
    text-align: center;
    padding: 4rem 1rem;
    color: var(--muted);
  }
  .cart-empty p {
    font-family: var(--display);
    font-style: italic;
    font-size: 1.3rem;
    margin-bottom: 1.5rem;
    color: var(--ink-soft);
  }
  .cart-footer {
    padding: 1.5rem;
    border-top: 1px solid var(--line);
    background: var(--paper);
  }
  .cart-summary {
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
    margin-bottom: 1.5rem;
  }
  .cart-row {
    display: flex;
    justify-content: space-between;
    font-size: 0.85rem;
    color: var(--ink-soft);
  }
  .cart-row.total {
    font-family: var(--display);
    font-size: 1.4rem;
    color: var(--ink);
    padding-top: 0.6rem;
    margin-top: 0.4rem;
    border-top: 1px solid var(--line);
  }
  .cart-row.total span:last-child { color: var(--burgundy); }
  .checkout-btn {
    width: 100%;
    padding: 1.1rem;
    background: var(--ink);
    color: var(--cream);
    border: none;
    font-family: var(--body);
    font-size: 0.75rem;
    letter-spacing: 0.28em;
    text-transform: uppercase;
    font-weight: 500;
    cursor: pointer;
    transition: background 0.3s;
  }
  .checkout-btn:hover:not(:disabled) { background: var(--burgundy); }
  .checkout-btn:disabled { opacity: 0.4; cursor: not-allowed; }
  .secure-note {
    text-align: center;
    margin-top: 1rem;
    font-size: 0.68rem;
    color: var(--muted);
    letter-spacing: 0.18em;
    text-transform: uppercase;
  }

  /* ========== MODAL ========== */
  .modal-overlay {
    position: fixed;
    inset: 0;
    background: rgba(31, 20, 13, 0.7);
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 300;
    padding: 1rem;
    backdrop-filter: blur(8px);
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--cream);
    max-width: 720px;
    width: 100%;
    max-height: 85vh;
    overflow-y: auto;
    padding: 3rem;
    position: relative;
    border: 1px solid var(--line-strong);
  }
  .modal h2 {
    font-family: var(--display);
    font-size: 2.2rem;
    margin-bottom: 0.4rem;
    font-weight: 400;
  }
  .modal h2 em { color: var(--burgundy); font-style: italic; }
  .modal-eyebrow {
    font-size: 0.7rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--burgundy);
    margin-bottom: 1rem;
    font-weight: 500;
  }
  .modal h3 {
    font-family: var(--display);
    margin-top: 2rem;
    margin-bottom: 0.5rem;
    color: var(--ink);
    font-weight: 500;
    font-size: 1.2rem;
  }
  .modal p {
    color: var(--ink-soft);
    font-size: 0.95rem;
    margin-bottom: 0.9rem;
    line-height: 1.8;
  }
  .modal-close {
    position: absolute;
    top: 1rem; right: 1rem;
    background: none;
    border: none;
    color: var(--ink);
    font-size: 1.5rem;
    cursor: pointer;
    width: 36px; height: 36px;
    line-height: 1;
  }
  .modal-close:hover { color: var(--burgundy); }

  /* Toast notification */
  .toast {
    position: fixed;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%) translateY(120%);
    background: var(--ink);
    color: var(--cream);
    padding: 1rem 2rem;
    font-size: 0.8rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    z-index: 250;
    transition: transform 0.5s cubic-bezier(0.65,0,0.35,1);
    border: 1px solid var(--gold);
  }
  .toast.show { transform: translateX(-50%) translateY(0); }
  .toast .gold { color: var(--gold); }

  /* Reveal on scroll */
  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.9s ease, transform 0.9s ease;
  }
  .reveal.shown {
    opacity: 1;
    transform: translateY(0);
  }
</style>
</head>
<body>

<!-- ========== NAVIGATION ========== -->
<nav>
  <div class="nav-left">
    <a href="#collection" class="nav-link">Collection</a>
    <a href="#bridal" class="nav-link">Bespoke</a>
    <a href="#atelier" class="nav-link">Atelier</a>
  </div>
  <div class="logo">J Beaver's <em>Jewels</em></div>
  <div class="nav-right">
    <a href="#contact" class="nav-link">Contact</a>
    <button class="cart-trigger" onclick="toggleCart()">
      Bag <span class="cart-count" id="cartCount">0</span>
    </button>
    <button class="mobile-toggle" onclick="toggleMobileMenu()" aria-label="Menu">
      <span></span><span></span><span></span>
    </button>
  </div>
</nav>

<div class="mobile-menu" id="mobileMenu">
  <a href="#collection" onclick="toggleMobileMenu()"><em>01.</em> Collection</a>
  <a href="#bridal" onclick="toggleMobileMenu()"><em>02.</em> Bespoke</a>
  <a href="#atelier" onclick="toggleMobileMenu()"><em>03.</em> Atelier</a>
  <a href="#faq" onclick="toggleMobileMenu()"><em>04.</em> FAQ</a>
  <a href="#contact" onclick="toggleMobileMenu()"><em>05.</em> Contact</a>
</div>

<!-- ========== HERO ========== -->
<section class="hero">
  <div class="hero-text">
    <div class="hero-eyebrow">Heirloom Fine Jewelry · Est. <span class="cfg-established"></span></div>
    <h1>Adorn. <em>With intention.</em></h1>
    <p class="hero-sub">Pieces forged in 18k gold, reclaimed sterling, and ethically sourced gemstones. Hand finished at our American atelier. Made to be loved, kept, and passed down.</p>
    <div class="hero-actions">
      <a href="#collection" class="btn btn-primary">Shop the Collection</a>
      <a href="#atelier" class="btn btn-secondary">Our Story</a>
    </div>
  </div>
  <div class="hero-visual">
    <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <linearGradient id="heroGold" x1="0" x2="1" y1="0" y2="1">
          <stop offset="0" stop-color="#d4a464"/>
          <stop offset="1" stop-color="#9a6f30"/>
        </linearGradient>
        <radialGradient id="heroGem">
          <stop offset="0" stop-color="#fff"/>
          <stop offset="0.5" stop-color="#f5ede1"/>
          <stop offset="1" stop-color="#b8884c"/>
        </radialGradient>
      </defs>
      <circle cx="100" cy="125" r="55" fill="none" stroke="url(#heroGold)" stroke-width="8"/>
      <path d="M100 40 L88 65 L112 65 Z" fill="url(#heroGem)" stroke="#d4a464" stroke-width="1"/>
      <circle cx="100" cy="72" r="14" fill="url(#heroGem)" stroke="#d4a464" stroke-width="1.5"/>
      <circle cx="95" cy="68" r="3" fill="#fff" opacity="0.9"/>
    </svg>
    <span class="hero-est">EST. <span class="cfg-established"></span></span>
  </div>
</section>

<!-- ========== MARQUEE ========== -->
<div class="marquee">
  <div class="marquee-track">
    <span>Ethically Sourced</span><span>Hand Finished in America</span><span>Lifetime Polishing</span><span>Free Insured Shipping</span><span>Certificate of Authenticity</span><span>Conflict Free Gemstones</span>
    <span>Ethically Sourced</span><span>Hand Finished in America</span><span>Lifetime Polishing</span><span>Free Insured Shipping</span><span>Certificate of Authenticity</span><span>Conflict Free Gemstones</span>
  </div>
</div>

<!-- ========== FEATURED TRIO ========== -->
<section class="featured" id="featured">
  <div class="container">
    <div class="section-header reveal">
      <div class="eyebrow">Pieces of the Season</div>
      <h2 class="section-title">Featured <em>this autumn</em></h2>
      <p class="section-desc">Three signature pieces, each finished entirely by hand. Limited quantities. Released first to our community.</p>
    </div>
    <div class="featured-grid" id="featuredGrid"></div>
  </div>
</section>

<!-- ========== COLLECTION ========== -->
<section id="collection">
  <div class="container">
    <div class="section-header reveal">
      <div class="eyebrow">The Collection</div>
      <h2 class="section-title">Pieces with <em>provenance</em></h2>
      <p class="section-desc">Every piece is hallmarked, certified, and crafted with archival materials. Filter by category to find your next forever piece.</p>
    </div>
    <div class="filters reveal" id="filters">
      <button class="filter-btn active" data-filter="all">All</button>
      <button class="filter-btn" data-filter="ring">Rings</button>
      <button class="filter-btn" data-filter="earring">Earrings</button>
      <button class="filter-btn" data-filter="necklace">Necklaces</button>
      <button class="filter-btn" data-filter="bracelet">Bracelets</button>
    </div>
    <div class="product-grid" id="productGrid"></div>
  </div>
</section>

<!-- ========== EDITORIAL SPLIT ========== -->
<section class="split" id="atelier" style="padding:0;">
  <div class="split-visual">
    <svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <linearGradient id="atGold" x1="0" x2="1">
          <stop offset="0" stop-color="#d4a464"/>
          <stop offset="1" stop-color="#9a6f30"/>
        </linearGradient>
      </defs>
      <circle cx="70" cy="100" r="35" fill="none" stroke="url(#atGold)" stroke-width="4"/>
      <circle cx="130" cy="100" r="35" fill="none" stroke="url(#atGold)" stroke-width="4"/>
      <path d="M105 100 L115 100" stroke="url(#atGold)" stroke-width="4"/>
      <circle cx="70" cy="100" r="4" fill="#d4a464"/>
      <circle cx="130" cy="100" r="4" fill="#d4a464"/>
    </svg>
  </div>
  <div class="split-text">
    <div class="eyebrow">The Atelier</div>
    <h2>Made by hand. <em>Made to outlive us.</em></h2>
    <p>J Beaver's Jewels began with a single goldsmith's bench and a single conviction: that fine jewelry should be made slowly, ethically, and to be passed down. We work in small batches with reclaimed metals and traceable stones.</p>
    <p>No mass production. No outsourcing. No shortcuts. Every piece is hallmarked, signed, and registered to its owner. We guarantee our work for life.</p>
    <div class="signature">— <span class="cfg-founder"></span>, Founder</div>
  </div>
</section>

<!-- ========== CRAFT PROCESS ========== -->
<section class="craft">
  <div class="container">
    <div class="section-header reveal">
      <div class="eyebrow">The Craft</div>
      <h2 class="section-title">From bench to <em>your hand</em></h2>
    </div>
    <div class="craft-grid">
      <div class="craft-step reveal">
        <span class="craft-num">i.</span>
        <h3>Sketch</h3>
        <p>Each piece begins as a hand drawn study at the bench. We refine the silhouette until it can be made no simpler.</p>
      </div>
      <div class="craft-step reveal">
        <span class="craft-num">ii.</span>
        <h3>Source</h3>
        <p>Metals are reclaimed and refined to 18k or 14k purity. Stones are conflict free and individually inspected.</p>
      </div>
      <div class="craft-step reveal">
        <span class="craft-num">iii.</span>
        <h3>Craft</h3>
        <p>The bench work is patient. Stones are hand set. Settings are filed and polished one facet at a time.</p>
      </div>
      <div class="craft-step reveal">
        <span class="craft-num">iv.</span>
        <h3>Finish</h3>
        <p>Each piece is hallmarked, registered, and packaged in a hand stamped pouch with its certificate.</p>
      </div>
    </div>
  </div>
</section>

<!-- ========== TESTIMONIALS ========== -->
<section>
  <div class="container">
    <div class="section-header reveal">
      <div class="eyebrow">In Their Words</div>
      <h2 class="section-title">Worn and <em>treasured</em></h2>
    </div>
    <div class="testimonial-grid">
      <div class="testimonial reveal">
        <div class="stars">★ ★ ★ ★ ★</div>
        <p class="testimonial-text">My engagement ring is the only piece of jewelry I have never taken off. The craftsmanship is extraordinary, and the team made the whole experience feel personal.</p>
        <div class="testimonial-author">Eleanor V. · New York</div>
      </div>
      <div class="testimonial reveal">
        <div class="stars">★ ★ ★ ★ ★</div>
        <p class="testimonial-text">I bought my mother the pearl drops for her birthday. The packaging alone made her cry. Then she opened the box.</p>
        <div class="testimonial-author">Marcus L. · San Francisco</div>
      </div>
      <div class="testimonial reveal">
        <div class="stars">★ ★ ★ ★ ★</div>
        <p class="testimonial-text">I have had pieces from much larger houses. None feels as personal or as well made as my Atlas signet. It is the heirloom my children will wear.</p>
        <div class="testimonial-author">Priya M. · Chicago</div>
      </div>
    </div>
  </div>
</section>

<!-- ========== BESPOKE BANNER ========== -->
<section class="bespoke" id="bridal">
  <div class="eyebrow">Bespoke &amp; Bridal</div>
  <h2>A piece made for <em>one person only.</em></h2>
  <p>From engagement rings to memorial pieces, we design and craft to commission. Initial consultations are complimentary and confidential.</p>
  <a href="#contact" class="btn btn-gold">Begin a Conversation</a>
</section>

<!-- ========== FAQ ========== -->
<section id="faq">
  <div class="container">
    <div class="section-header reveal">
      <div class="eyebrow">Frequently Asked</div>
      <h2 class="section-title">Good <em>questions</em></h2>
    </div>
    <div class="faq-list">
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">What metals do you work in?</button>
        <div class="faq-a"><p>We work primarily in 18k yellow, white, and rose gold, 14k gold for everyday pieces, platinum on request, and 925 sterling silver. All metals are reclaimed or sourced from refiners with full chain of custody documentation.</p></div>
      </div>
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">Are your gemstones ethically sourced?</button>
        <div class="faq-a"><p>Yes. Every gemstone is conflict free and accompanied by origin documentation where possible. We work with cutters and suppliers we have personally vetted. For diamonds, we offer both natural and laboratory grown options with full grading reports.</p></div>
      </div>
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">How do I find my ring size?</button>
        <div class="faq-a"><p>We can post a complimentary ring sizer kit anywhere in the United States. Email the concierge with your address and we will dispatch within the day. For international orders, we provide a printable sizer.</p></div>
      </div>
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">Do you offer engraving?</button>
        <div class="faq-a"><p>Yes. Hand engraving and laser engraving are available on most pieces. Hand engraving carries a lead time of two to three weeks. Engraved pieces are final sale.</p></div>
      </div>
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">How do I care for my piece?</button>
        <div class="faq-a"><p>Store each piece separately in its pouch. Avoid lotion, perfume, and chlorinated water. We offer complimentary lifetime polishing for original owners. Post your piece in the insured pouch we provide and we will return it refreshed within two weeks.</p></div>
      </div>
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">Do you ship internationally?</button>
        <div class="faq-a"><p>Yes. We ship worldwide via fully insured express courier. Domestic United States delivery is complimentary and arrives within 3 to 5 business days. International delivery varies by region and typically takes 7 to 14 business days.</p></div>
      </div>
      <div class="faq-item">
        <button class="faq-q" onclick="toggleFaq(this)">What is your return policy?</button>
        <div class="faq-a"><p>Unworn pieces in their original packaging may be returned within 30 days for a full refund. Engraved, bespoke, and final sale pieces are not eligible. See our full Refund Policy in the footer.</p></div>
      </div>
    </div>
  </div>
</section>

<!-- ========== NEWSLETTER ========== -->
<section class="newsletter">
  <div class="container">
    <div class="eyebrow">The Journal</div>
    <h2 class="section-title">Quiet <em>correspondence</em></h2>
    <p class="section-desc">First access to new releases, atelier stories, and private events. We write rarely and never sell your address.</p>
    <form class="newsletter-form" onsubmit="subscribeNewsletter(event)">
      <input type="email" placeholder="your@email.com" required id="newsletterEmail" />
      <button type="submit">Subscribe</button>
    </form>
    <div class="newsletter-success" id="newsletterSuccess">Thank you. We will be in touch.</div>
  </div>
</section>

<!-- ========== CONTACT ========== -->
<section class="contact" id="contact">
  <div class="container">
    <div class="section-header reveal">
      <div class="eyebrow">In Conversation</div>
      <h2 class="section-title">Speak with <em>the atelier</em></h2>
      <p class="section-desc">Reach us any hour. We respond to most enquiries within the day.</p>
    </div>
    <div class="contact-grid">
      <div class="contact-card">
        <div class="icon">&#9993;</div>
        <h5>Concierge</h5>
        <p><a class="cfg-email-link"></a></p>
      </div>
      <div class="contact-card">
        <div class="icon">&#9743;</div>
        <h5>Telephone</h5>
        <p><a class="cfg-phone-link"></a></p>
      </div>
      <div class="contact-card">
        <div class="icon">&#9728;</div>
        <h5>Hours</h5>
        <p><span class="cfg-hours-1"></span><br/><span class="cfg-hours-2"></span></p>
      </div>
    </div>
  </div>
</section>

<!-- ========== FOOTER ========== -->
<footer>
  <div class="footer-grid">
    <div class="footer-brand">
      <div class="logo">J Beaver's <em>Jewels</em></div>
      <p>Heirloom fine jewelry, hand finished in the United States. Ethically sourced. Made to be passed down.</p>
    </div>
    <div class="footer-col">
      <h6>Shop</h6>
      <a href="#collection">All Pieces</a>
      <a href="#collection">Rings</a>
      <a href="#collection">Earrings</a>
      <a href="#collection">Necklaces</a>
      <a href="#bridal">Bespoke</a>
    </div>
    <div class="footer-col">
      <h6>House</h6>
      <a href="#atelier">The Atelier</a>
      <a href="#faq">FAQ</a>
      <a href="#contact">Contact</a>
    </div>
    <div class="footer-col">
      <h6>Policies</h6>
      <a href="#" onclick="openModal('terms'); return false;">Terms of Service</a>
      <a href="#" onclick="openModal('privacy'); return false;">Privacy Policy</a>
      <a href="#" onclick="openModal('refunds'); return false;">Refund Policy</a>
      <a href="#" onclick="openModal('shipping'); return false;">Shipping Policy</a>
    </div>
  </div>
  <div class="footer-bottom">
    <p>&copy; <span class="cfg-year"></span> <span class="cfg-business-name"></span>. All rights reserved. Payments secured by <span class="stripe-mark">STRIPE</span> · SSL Encrypted · PCI Compliant</p>
  </div>
</footer>

<!-- ========== CART DRAWER ========== -->
<div class="overlay" id="overlay" onclick="closeAll()"></div>
<aside class="cart-drawer" id="cartDrawer">
  <div class="cart-header">
    <h3>Your Bag</h3>
    <button class="icon-btn" onclick="toggleCart()" aria-label="Close">&times;</button>
  </div>
  <div class="cart-items" id="cartItems"></div>
  <div class="cart-footer">
    <div class="cart-summary">
      <div class="cart-row"><span>Subtotal</span><span id="cartSubtotal">$0.00</span></div>
      <div class="cart-row"><span>Insured shipping</span><span>Calculated at checkout</span></div>
      <div class="cart-row total"><span>Total</span><span id="cartTotal">$0.00</span></div>
    </div>
    <button class="checkout-btn" id="checkoutBtn" onclick="checkout()">Secure Checkout</button>
    <p class="secure-note">Powered by Stripe</p>
  </div>
</aside>

<!-- ========== POLICY MODALS ========== -->
<div class="modal-overlay" id="modalOverlay" onclick="if(event.target===this)closeModal()">
  <div class="modal">
    <button class="modal-close" onclick="closeModal()" aria-label="Close">&times;</button>
    <div id="modalContent"></div>
  </div>
</div>

<!-- ========== TOAST ========== -->
<div class="toast" id="toast"><span class="gold">&#10003;</span> <span id="toastMsg">Added to bag</span></div>

<script>
  // ============ SITE CONFIG ============
  // All business details (email, phone, hours, etc.) live in config.js.
  // To change them, edit public/config.js — never edit values directly here.
  const SITE_CONFIG = window.SITE_CONFIG || {};

  function applyConfig() {
    const c = SITE_CONFIG;
    const fills = {
      "cfg-business-name": c.businessName,
      "cfg-email": c.email,
      "cfg-phone": c.phone,
      "cfg-hours-1": c.hoursLine1,
      "cfg-hours-2": c.hoursLine2,
      "cfg-year": c.copyrightYear,
      "cfg-established": c.established,
      "cfg-founder": c.founder,
    };
    Object.entries(fills).forEach(([cls, val]) => {
      if (val !== undefined && val !== null) {
        document.querySelectorAll("." + cls).forEach(el => { el.textContent = val; });
      }
    });
    if (c.email) {
      document.querySelectorAll(".cfg-email-link").forEach(el => {
        el.href = "mailto:" + c.email;
        if (!el.textContent.trim()) el.textContent = c.email;
      });
    }
    if (c.phoneRaw) {
      document.querySelectorAll(".cfg-phone-link").forEach(el => {
        el.href = "tel:" + c.phoneRaw;
        if (!el.textContent.trim()) el.textContent = c.phone || c.phoneRaw;
      });
    }
  }

  // ============ PRODUCT CATALOG ============
  // Prices in cents. $245.00 = 24500
  const PRODUCTS = [
    {
      id: "ember-solitaire", name: "Ember Solitaire", category: "ring", tag: "Signature",
      description: "18k yellow gold band set with a half carat brilliant cut white sapphire.",
      price: 24500, featured: true, badge: "Signature",
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p1g" x1="0" x2="1"><stop offset="0" stop-color="#d4a464"/><stop offset="1" stop-color="#9a6f30"/></linearGradient></defs><circle cx="50" cy="62" r="26" fill="none" stroke="url(#p1g)" stroke-width="5"/><path d="M50 25 L43 38 L57 38 Z" fill="#fbf6ec" stroke="#b8884c" stroke-width="0.8"/><circle cx="50" cy="42" r="7" fill="#fbf6ec" stroke="#b8884c" stroke-width="0.8"/><circle cx="48" cy="40" r="2" fill="#fff"/></svg>'
    },
    {
      id: "vesper-tennis", name: "Vesper Tennis Bracelet", category: "bracelet", tag: "Heirloom",
      description: "Lab grown diamond tennis bracelet, two carat total weight, 14k white gold.",
      price: 67500, featured: true, badge: "Heirloom",
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p2g" x1="0" x2="1"><stop offset="0" stop-color="#e8e8e8"/><stop offset="1" stop-color="#a8a8a8"/></linearGradient></defs><ellipse cx="50" cy="50" rx="38" ry="8" fill="none" stroke="url(#p2g)" stroke-width="2"/><g fill="url(#p2g)" stroke="#fff" stroke-width="0.5"><rect x="18" y="46" width="6" height="8"/><rect x="28" y="46" width="6" height="8"/><rect x="38" y="46" width="6" height="8"/><rect x="48" y="46" width="6" height="8"/><rect x="58" y="46" width="6" height="8"/><rect x="68" y="46" width="6" height="8"/></g></svg>'
    },
    {
      id: "aurelia-pearl", name: "Aurelia Pearl Drops", category: "earring", tag: "Bridal",
      description: "Akoya cultured pearls suspended from 18k gold hooks. Sold as a hallmarked pair.",
      price: 32500, featured: true, badge: "Bridal",
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><radialGradient id="p3p"><stop offset="0" stop-color="#fffaf0"/><stop offset="0.5" stop-color="#f0e4d0"/><stop offset="1" stop-color="#c9b890"/></radialGradient></defs><path d="M35 18 Q30 25 32 35" stroke="#b8884c" stroke-width="2" fill="none"/><circle cx="35" cy="57" r="13" fill="url(#p3p)"/><circle cx="32" cy="53" r="3" fill="#fff" opacity="0.7"/><path d="M65 18 Q70 25 68 35" stroke="#b8884c" stroke-width="2" fill="none"/><circle cx="65" cy="57" r="13" fill="url(#p3p)"/><circle cx="62" cy="53" r="3" fill="#fff" opacity="0.7"/></svg>'
    },
    {
      id: "lumiere-chain", name: "Lumiere Rope Chain", category: "necklace", tag: "Everyday",
      description: "Reclaimed 14k gold rope chain, eighteen inches, with lobster clasp and maker's mark.",
      price: 18900,
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p4g" x1="0" x2="1"><stop offset="0" stop-color="#d4a464"/><stop offset="1" stop-color="#8a6230"/></linearGradient></defs><path d="M15 50 Q25 38 35 50 T55 50 T75 50 T95 50" fill="none" stroke="url(#p4g)" stroke-width="4" stroke-linecap="round"/><path d="M15 58 Q25 46 35 58 T55 58 T75 58 T95 58" fill="none" stroke="url(#p4g)" stroke-width="4" stroke-linecap="round"/><circle cx="15" cy="54" r="3" fill="#b8884c"/><circle cx="95" cy="54" r="3" fill="#b8884c"/></svg>'
    },
    {
      id: "meridian-bangle", name: "Meridian Bangle", category: "bracelet", tag: "Sculpted",
      description: "Sterling silver bangle with brushed exterior and high polished interior. One size.",
      price: 16500,
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p5g" x1="0" x2="1"><stop offset="0" stop-color="#e8e8e8"/><stop offset="0.5" stop-color="#c0c0c0"/><stop offset="1" stop-color="#8a8a8a"/></linearGradient></defs><ellipse cx="50" cy="50" rx="36" ry="32" fill="none" stroke="url(#p5g)" stroke-width="7"/><ellipse cx="50" cy="50" rx="30" ry="26" fill="none" stroke="#fbf6ec" stroke-width="1"/></svg>'
    },
    {
      id: "north-star", name: "North Star Pendant", category: "necklace", tag: "Talisman",
      description: "Eight pointed star carved in 14k gold on a fine sixteen inch cable chain.",
      price: 21500,
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p6g" x1="0" x2="1"><stop offset="0" stop-color="#d4a464"/><stop offset="1" stop-color="#9a6f30"/></linearGradient></defs><path d="M15 45 Q35 38 50 45" stroke="#b8884c" stroke-width="1.5" fill="none"/><path d="M50 45 Q65 38 85 45" stroke="#b8884c" stroke-width="1.5" fill="none"/><path d="M50 32 L56 50 L74 56 L56 62 L50 80 L44 62 L26 56 L44 50 Z" fill="url(#p6g)" stroke="#8a6230" stroke-width="0.5"/><circle cx="50" cy="56" r="3" fill="#fff" opacity="0.7"/></svg>'
    },
    {
      id: "atlas-cuff", name: "Atlas Signet Cuff", category: "bracelet", tag: "Heritage",
      description: "Heavy 925 sterling cuff with hand engraved signet face suitable for monogramming.",
      price: 29500,
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p7g" x1="0" x2="1"><stop offset="0" stop-color="#e0e0e0"/><stop offset="1" stop-color="#909090"/></linearGradient></defs><path d="M22 42 Q50 26 78 42 L78 62 Q50 78 22 62 Z" fill="url(#p7g)" stroke="#707070" stroke-width="0.8"/><rect x="40" y="44" width="20" height="16" fill="#5a5a5a" opacity="0.4"/><text x="50" y="57" text-anchor="middle" fill="#b8884c" font-family="serif" font-size="11" font-style="italic" font-weight="600">JB</text></svg>'
    },
    {
      id: "constellation-studs", name: "Constellation Studs", category: "earring", tag: "Classic",
      description: "Half carat each lab grown diamond studs in 14k white gold martini settings.",
      price: 38500,
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><radialGradient id="p8d"><stop offset="0" stop-color="#fff"/><stop offset="0.7" stop-color="#e8e8e8"/><stop offset="1" stop-color="#a0a0a0"/></radialGradient></defs><polygon points="32,45 24,55 32,68 40,55" fill="url(#p8d)" stroke="#909090" stroke-width="0.5"/><polygon points="68,45 60,55 68,68 76,55" fill="url(#p8d)" stroke="#909090" stroke-width="0.5"/><polygon points="32,45 32,55 24,55" fill="#fff" opacity="0.7"/><polygon points="68,45 68,55 60,55" fill="#fff" opacity="0.7"/></svg>'
    },
    {
      id: "hadley-hoops", name: "Hadley Hoop Earrings", category: "earring", tag: "Everyday",
      description: "Petite 14k gold hoops with hand polished finish. Suitable for daily wear.",
      price: 14500,
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p9g" x1="0" x2="1"><stop offset="0" stop-color="#d4a464"/><stop offset="1" stop-color="#9a6f30"/></linearGradient></defs><circle cx="32" cy="50" r="18" fill="none" stroke="url(#p9g)" stroke-width="4"/><circle cx="68" cy="50" r="18" fill="none" stroke="url(#p9g)" stroke-width="4"/></svg>'
    },
    {
      id: "saxon-bridal", name: "Saxon Bridal Set", category: "ring", tag: "Bridal",
      description: "Matched engagement ring and wedding band set, 18k gold with one carat center stone.",
      price: 89500, badge: "Bridal",
      svg: '<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="p10g" x1="0" x2="1"><stop offset="0" stop-color="#d4a464"/><stop offset="1" stop-color="#9a6f30"/></linearGradient></defs><circle cx="40" cy="60" r="22" fill="none" stroke="url(#p10g)" stroke-width="4"/><circle cx="65" cy="60" r="22" fill="none" stroke="url(#p10g)" stroke-width="3"/><path d="M40 30 L34 42 L46 42 Z" fill="#fbf6ec" stroke="#b8884c" stroke-width="0.6"/><circle cx="40" cy="45" r="6" fill="#fbf6ec" stroke="#b8884c"/></svg>'
    }
  ];

  // ============ STATE ============
  const cart = {};
  let currentFilter = "all";
  const fmt = (cents) => "$" + (cents / 100).toFixed(2);

  // ============ RENDER FEATURED ============
  function renderFeatured() {
    const grid = document.getElementById("featuredGrid");
    grid.innerHTML = PRODUCTS.filter(p => p.featured).map(p => `
      <div class="featured-card reveal" onclick="document.getElementById('collection').scrollIntoView({behavior:'smooth'})">
        <div class="featured-image">${p.svg}</div>
        <div class="featured-name">${p.name}</div>
        <div class="featured-price">${fmt(p.price)}</div>
      </div>
    `).join("");
  }

  // ============ RENDER PRODUCTS ============
  function renderProducts() {
    const grid = document.getElementById("productGrid");
    const filtered = currentFilter === "all"
      ? PRODUCTS
      : PRODUCTS.filter(p => p.category === currentFilter);
    grid.innerHTML = filtered.map(p => `
      <div class="product" data-id="${p.id}" data-cat="${p.category}">
        <div class="product-image">
          ${p.badge ? `<div class="product-badge">${p.badge}</div>` : ""}
          ${p.svg}
        </div>
        <div class="product-info">
          <div class="product-cat">${p.tag}</div>
          <h3 class="product-name">${p.name}</h3>
          <p class="product-desc">${p.description}</p>
          <div class="product-bottom">
            <div class="product-price">${fmt(p.price)}</div>
            <button class="add-btn" onclick="event.stopPropagation(); addToCart('${p.id}')">Add to Bag</button>
          </div>
        </div>
      </div>
    `).join("");
    initReveal();
  }

  // ============ FILTERS ============
  document.querySelectorAll(".filter-btn").forEach(btn => {
    btn.addEventListener("click", () => {
      document.querySelectorAll(".filter-btn").forEach(b => b.classList.remove("active"));
      btn.classList.add("active");
      currentFilter = btn.dataset.filter;
      renderProducts();
    });
  });

  // ============ CART ============
  function addToCart(id) {
    cart[id] = (cart[id] || 0) + 1;
    renderCart();
    const product = PRODUCTS.find(p => p.id === id);
    showToast(product.name + " added");
    const btn = document.querySelector(`.product[data-id="${id}"] .add-btn`);
    if (btn) {
      btn.classList.add("added");
      btn.textContent = "Added \u2713";
      setTimeout(() => {
        btn.classList.remove("added");
        btn.textContent = "Add to Bag";
      }, 1600);
    }
    const countEl = document.getElementById("cartCount");
    countEl.classList.add("bump");
    setTimeout(() => countEl.classList.remove("bump"), 300);
  }

  function changeQty(id, delta) {
    cart[id] = (cart[id] || 0) + delta;
    if (cart[id] <= 0) delete cart[id];
    renderCart();
  }

  function removeFromCart(id) {
    delete cart[id];
    renderCart();
  }

  function renderCart() {
    const itemsEl = document.getElementById("cartItems");
    const countEl = document.getElementById("cartCount");
    const subEl = document.getElementById("cartSubtotal");
    const totalEl = document.getElementById("cartTotal");
    const checkoutBtn = document.getElementById("checkoutBtn");
    const ids = Object.keys(cart);

    let total = 0;
    let itemCount = 0;

    if (ids.length === 0) {
      itemsEl.innerHTML = `
        <div class="cart-empty">
          <p>Your bag awaits.</p>
          <button class="btn btn-secondary" onclick="toggleCart()">Browse the Collection</button>
        </div>`;
    } else {
      itemsEl.innerHTML = ids.map(id => {
        const p = PRODUCTS.find(x => x.id === id);
        const qty = cart[id];
        total += p.price * qty;
        itemCount += qty;
        return `
          <div class="cart-item">
            <div class="cart-item-img">${p.svg}</div>
            <div class="cart-item-info">
              <div class="cart-item-top">
                <div class="cart-item-name">${p.name}</div>
                <button class="cart-item-remove" onclick="removeFromCart('${id}')">Remove</button>
              </div>
              <div class="cart-item-price">${fmt(p.price)}</div>
              <div class="qty">
                <button onclick="changeQty('${id}', -1)" aria-label="Decrease">&minus;</button>
                <span>${qty}</span>
                <button onclick="changeQty('${id}', 1)" aria-label="Increase">+</button>
              </div>
            </div>
          </div>
        `;
      }).join("");
    }
    countEl.textContent = itemCount;
    subEl.textContent = fmt(total);
    totalEl.textContent = fmt(total);
    checkoutBtn.disabled = ids.length === 0;
  }

  function toggleCart() {
    document.getElementById("cartDrawer").classList.toggle("open");
    document.getElementById("overlay").classList.toggle("open");
  }

  function toggleMobileMenu() {
    document.getElementById("mobileMenu").classList.toggle("open");
  }

  function closeAll() {
    document.getElementById("cartDrawer").classList.remove("open");
    document.getElementById("overlay").classList.remove("open");
    document.getElementById("mobileMenu").classList.remove("open");
  }

  // ============ TOAST ============
  function showToast(msg) {
    const toast = document.getElementById("toast");
    document.getElementById("toastMsg").textContent = msg;
    toast.classList.add("show");
    clearTimeout(window._toastTimer);
    window._toastTimer = setTimeout(() => toast.classList.remove("show"), 2500);
  }

  // ============ STRIPE CHECKOUT ============
  async function checkout() {
    const btn = document.getElementById("checkoutBtn");
    btn.disabled = true;
    btn.textContent = "Preparing checkout...";
    try {
      const items = Object.keys(cart).map(id => ({ id, quantity: cart[id] }));
      const res = await fetch("/create-checkout-session", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ items })
      });
      if (!res.ok) {
        const errData = await res.json().catch(() => ({}));
        throw new Error(errData.error || "Checkout failed. Please try again.");
      }
      const data = await res.json();
      if (data.url) {
        window.location.href = data.url;
      } else {
        throw new Error(data.error || "Checkout failed.");
      }
    } catch (err) {
      showToast(err.message);
      btn.disabled = false;
      btn.textContent = "Secure Checkout";
    }
  }

  // ============ FAQ ============
  function toggleFaq(btn) {
    const item = btn.closest(".faq-item");
    item.classList.toggle("open");
  }

  // ============ NEWSLETTER ============
  function subscribeNewsletter(e) {
    e.preventDefault();
    const email = document.getElementById("newsletterEmail").value;
    // In production, POST this to your backend or a service like Mailchimp
    console.log("Newsletter signup:", email);
    document.getElementById("newsletterEmail").value = "";
    document.getElementById("newsletterSuccess").classList.add("show");
    setTimeout(() => {
      document.getElementById("newsletterSuccess").classList.remove("show");
    }, 4000);
  }

  // ============ POLICY MODALS ============
  const POLICIES = {
    terms: {
      eyebrow: "Legal",
      title: "Terms of <em>Service</em>",
      body: `
        <p>Welcome to J Beaver's Jewels. These Terms govern your use of our website and your purchase of our products. By accessing the site or placing an order, you agree to these Terms.</p>
        <h3>1. Use of the Site</h3>
        <p>You agree to use this website only for lawful purposes and in a way that does not infringe the rights of others or restrict their use of the site. You must be at least 18 years old to make a purchase.</p>
        <h3>2. Orders and Pricing</h3>
        <p>All prices are listed in United States Dollars and are exclusive of any duties or taxes that may apply to international orders. We reserve the right to refuse, cancel, or modify any order at our sole discretion, including in cases of pricing errors or stock unavailability.</p>
        <h3>3. Payment</h3>
        <p>Payments are processed securely by Stripe, Inc. We do not store credit card information on our servers. All payment information is encrypted in transit and at rest by Stripe in accordance with PCI DSS standards.</p>
        <h3>4. Authenticity and Materials</h3>
        <p>Every piece sold by J Beaver's Jewels is accompanied by a certificate of authenticity stating metal purity, gemstone origin where applicable, and carat weight. Pieces are warranted to be as described.</p>
        <h3>5. Intellectual Property</h3>
        <p>All content, designs, photographs, and trademarks on this site are the property of J Beaver's Jewels and may not be reproduced, distributed, or used without our written permission.</p>
        <h3>6. Limitation of Liability</h3>
        <p>To the maximum extent permitted by law, J Beaver's Jewels shall not be liable for indirect, incidental, consequential, or punitive damages arising from the use of our products or website. Our maximum liability is limited to the amount paid for the product in question.</p>
        <h3>7. Governing Law</h3>
        <p>These Terms are governed by the laws of the United States. Any dispute arising under these Terms shall be resolved in the courts of the United States.</p>
        <h3>8. Contact</h3>
        <p>For questions regarding these Terms, contact us at ${SITE_CONFIG.email} or ${SITE_CONFIG.phone}.</p>
        <p style="font-size: 0.8rem; opacity: 0.7; margin-top: 2rem;">Last updated: 20 May 2026</p>
      `
    },
    privacy: {
      eyebrow: "Your Data",
      title: "Privacy <em>Policy</em>",
      body: `
        <p>J Beaver's Jewels respects your privacy. This policy describes what personal information we collect, how we use it, and the rights you have over it.</p>
        <h3>Information We Collect</h3>
        <p>When you make a purchase or contact us, we collect your name, email address, shipping and billing addresses, telephone number, and order history. Payment information is collected and processed by Stripe and is never stored on our servers.</p>
        <h3>How We Use Your Information</h3>
        <p>We use your information to process orders, ship products, respond to enquiries, send order updates and shipment tracking, and improve our service. With your consent, we may also send occasional newsletters about new collections and events.</p>
        <h3>Sharing of Information</h3>
        <p>We do not sell or rent your personal information. We share information only with service providers necessary to fulfill your order, including Stripe for payment processing and shipping carriers for delivery.</p>
        <h3>Cookies and Tracking</h3>
        <p>We use essential cookies to remember your bag contents and improve your browsing experience. You may disable cookies in your browser settings, though some site features may not function fully without them.</p>
        <h3>Your Rights</h3>
        <p>You may request access to, correction of, deletion of, or a copy of your personal data at any time by emailing ${SITE_CONFIG.email}. We will respond within 30 days. Residents of California and the European Union have additional rights under the CCPA and GDPR respectively.</p>
        <h3>Security</h3>
        <p>We use industry standard SSL encryption for all data in transit. Stripe handles all payment information under PCI DSS Level 1 compliance, the highest level available.</p>
        <h3>Children</h3>
        <p>Our site is not directed to children under the age of 13, and we do not knowingly collect personal information from children.</p>
        <h3>Contact</h3>
        <p>Questions about this policy may be directed to ${SITE_CONFIG.email} or ${SITE_CONFIG.phone}.</p>
        <p style="font-size: 0.8rem; opacity: 0.7; margin-top: 2rem;">Last updated: 20 May 2026</p>
      `
    },
    refunds: {
      eyebrow: "Returns",
      title: "Refund <em>Policy</em>",
      body: `
        <h3>Return Window</h3>
        <p>You may return any unworn piece in its original packaging within 30 days of delivery for a full refund of the product price. Original shipping is non refundable except in cases of our error.</p>
        <h3>How to Return</h3>
        <p>Email ${SITE_CONFIG.email} with your order number and reason for return. We will issue a prepaid insured return label within one business day. Place the piece in its original pouch and box, attach the label, and drop the parcel at any courier point.</p>
        <h3>Refund Timing</h3>
        <p>Refunds are issued to the original payment method within 5 to 10 business days of receiving and inspecting your return. Your bank may take additional time to post the credit.</p>
        <h3>Exceptions</h3>
        <p>The following pieces are final sale and cannot be returned unless they arrive damaged or defective: engraved pieces, custom sized pieces, bespoke commissions, earrings for hygiene reasons unless unopened, and any item marked Final Sale at purchase.</p>
        <h3>Damaged or Incorrect Items</h3>
        <p>If your order arrives damaged or you receive the wrong item, contact us within 7 days of delivery. We will arrange a replacement or full refund including all shipping costs.</p>
        <h3>Exchanges</h3>
        <p>For size or piece exchanges, place a new order and return the original within 30 days. This ensures you receive the new piece as quickly as possible.</p>
        <h3>Contact</h3>
        <p>Returns concierge: ${SITE_CONFIG.email} or ${SITE_CONFIG.phone}.</p>
      `
    },
    shipping: {
      eyebrow: "Delivery",
      title: "Shipping <em>Policy</em>",
      body: `
        <h3>Domestic Shipping (United States)</h3>
        <p>Complimentary fully insured ground shipping on every order. Delivery within 3 to 5 business days from dispatch. Express overnight delivery is available for $25.00 at checkout.</p>
        <h3>International Shipping</h3>
        <p>We ship worldwide via fully insured express courier. International delivery typically takes 7 to 14 business days. Shipping is calculated at checkout based on destination. The recipient is responsible for any duties, taxes, or customs fees levied by the destination country.</p>
        <h3>Processing Time</h3>
        <p>In stock pieces are dispatched within 1 to 2 business days. Made to order pieces require 2 to 4 weeks. Bespoke commissions require 6 to 12 weeks depending on complexity. We will email you with timing at the time of order.</p>
        <h3>Tracking</h3>
        <p>You will receive a tracking number by email at the moment of dispatch. All shipments are fully insured by us until they reach your hands.</p>
        <h3>Signature on Delivery</h3>
        <p>For orders over $500, a signature is required upon delivery for insurance purposes. Please ensure someone is available to receive your parcel.</p>
        <h3>Contact</h3>
        <p>Shipping concierge: ${SITE_CONFIG.email} or ${SITE_CONFIG.phone}.</p>
      `
    }
  };

  function openModal(key) {
    const p = POLICIES[key];
    document.getElementById("modalContent").innerHTML = `
      <div class="modal-eyebrow">${p.eyebrow}</div>
      <h2>${p.title}</h2>
      ${p.body}
    `;
    document.getElementById("modalOverlay").classList.add("open");
    document.body.style.overflow = "hidden";
  }
  function closeModal() {
    document.getElementById("modalOverlay").classList.remove("open");
    document.body.style.overflow = "";
  }

  // ============ SCROLL REVEAL ============
  function initReveal() {
    const els = document.querySelectorAll(".reveal:not(.shown)");
    const observer = new IntersectionObserver(entries => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add("shown");
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.1, rootMargin: "0px 0px -50px 0px" });
    els.forEach(el => observer.observe(el));
  }

  // ============ INIT ============
  applyConfig();
  renderFeatured();
  renderProducts();
  renderCart();
  initReveal();

  // Show cancelled message if returning from Stripe cancel
  if (window.location.search.includes("cancelled=1")) {
    setTimeout(() => showToast("Checkout cancelled. Your bag is preserved."), 500);
  }
</script>

</body>
</html>
