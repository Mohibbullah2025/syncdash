                             Professional Notes For HTML-5 & CSS

# Part 1: HTML5 Architecture & Semantics (The Core Backbone)

**১. সেম্যান্টিক কোডিং এবং "Div-itis" থেকে মুক্তি**
নতুন বা মিড-লেভেল ডেভেলপাররা প্রায়ই সব জায়গায় <div> ব্যবহার করেন। একে বলা হয় Div-itis। ব্রাউজার এবং এসইও (SEO) ক্রলারের কাছে <div> এর কোনো অর্থ নেই।

ভুল অ্যাপ্রোচ:

HTML
`<div class="header">...</div>
<div class="sidebar">...</div>`

প্রফেশনাল আর্কিটেকচার:

HTML
`<header role="banner">...</header>
<main>
        <section id="features">
        <article>...</article> </section>
        <aside>...</aside> </main>
        <footer>...</footer>
</main> `     

**২. অ্যাক্সেসিবিলিটি (A11y) এবং ARIA Attributes**

বিশ্বমানের প্রজেক্টগুলোতে প্রতিবন্ধী ব্যক্তিরা যাতে স্ক্রিন রিডার দিয়ে সাইট ব্রাউজ করতে পারেন, তা নিশ্চিত করা বাধ্যতামূলক।

Image Alt Text: আলংকারিক ইমেজের জন্য alt="" ফাঁকা রাখুন, কিন্তু গুরুত্বপূর্ণ ইমেজের জন্য বর্ণনামূলক টেক্সট দিন।

ARIA Roles: যদি কোনো বাটনকে আমরা <div> দিয়ে কাস্টমাইজ করি, তবে স্ক্রিন রিডারকে বোঝানোর জন্য role="button" এবং tabindex="0" ব্যবহার করতে হয়, যাতে কীবোর্ড দিয়ে সেটি সিলেক্ট করা যায়।

# Part 2: CSS Architecture, Specificity Wars & Optimization

**১. CSS Specificity (সুনির্দিষ্টতার যুদ্ধ)**

সিএসএস-এ যখন একাধিক রুল একই এলিমেন্টকে টার্গেট করে, তখন ব্রাউজার Specificity Score হিসাব করে সিদ্ধান্ত নেয় কোন স্টাইলটি দেখাবে।

Selector Type	       Example	         Specificity Score

Inline Style	  style="color: red;"	    1, 0, 0, 0

ID Selector            #sidebar	            0, 1, 0, 0

Class / Attribute   .card, [type="text"],   0, 0, 1, 0
/ Pseudo-class  	 :hover	

Element /           div, p, ::before        0, 0, 0, 1
Pseudo-element		

Universal Selector	      *	                0, 0, 0, 0
 
সতর্কতা: কোডে কখনো অলসতার কারণে !important ব্যবহার করবেন না। এটি সিএসএস এর স্পেসিফিসিটি চেইনকে ভেঙে ফেলে এবং পরবর্তীতে প্রজেক্ট মেইনটেইন করা অসম্ভব করে তোলে।

**২. CSS মেথডোলজি: BEM (Block, Element, Modifier)**

প্রফেশনাল ডেভেলপাররা ক্লাসের নাম মনগড়া না দিয়ে BEM আর্কিটেকচার মেনে চলেন। এর ফলে কোড দেখেই বোঝা যায় কোনটা কার কন্টেন্ট।

Block (.card): একটি স্বাধীন প্যারেন্ট কন্টেইনার।

Element (.card__title): ব্লকের ভেতরের অংশ, যা ব্লকের বাইরে এককভাবে চলতে পারে না (Double Underscore)।

Modifier (.card__button--disabled): ব্লক বা এলিমেন্টের কোনো স্টেট বা স্টাইল পরিবর্তন (Double Hyphen)।

Part 3: Deep Dive into Layout Engines (Flexbox vs Grid)
১. Flexbox Alignment Tricks
ফ্লেক্সবক্স ১-ডাইমেনশনাল (শুধু রো বা কলাম) লেআউটের রাজা। এর ৩টি প্রোপার্টি সবচেয়ে বেশি কনফিউশন তৈরি করে:

flex-grow: ফাঁকা জায়গা থাকলে এলিমেন্টটি কতটা বড় হবে (ডিফল্ট: 0)।

flex-shrink: জায়গা কম থাকলে এলিমেন্টটি কতটা সংকুচিত হবে (ডিফল্ট: 1)।

flex-basis: ফ্লেক্স এলিমেন্টের প্রাথমিক ডিফল্ট সাইজ (উইডথ বা হাইট নির্ধারণের প্রফেশনাল রূপ)।

CSS
.flex-item {
    /* shorthand: grow | shrink | basis */
    flex: 1 0 250px; 
}
২. CSS Grid (Auto-Fit বনাম Auto-Fill)
গ্রিড হলো ২-ডাইমেনশনাল। কোনো মিডিয়া কোয়েরি ছাড়া স্বয়ংক্রিয় রেসপনসিভ গ্রিড বানানোর সবচেয়ে শক্তিশালী অস্ত্র হলো minmax()।

auto-fill: গ্রিড ট্র্যাকের সংখ্যা বাড়াতে থাকে, भलेই ট্র্যাকগুলো খালি থাকুক।

auto-fit: খালি ট্র্যাকগুলোকে ভেঙে ফেলে বিদ্যমান আইটেমগুলোকে পুরো জায়গা জুড়ে বড় করে দেয়।

CSS
.grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1.5rem;
}
৩. CSS Container Queries (The Next Gen)
মিডিয়া কোয়েরি কাজ করে পুরো স্ক্রিনের (Viewport) ওপর ভিত্তি করে। কিন্তু মডার্ন সিএসএস-এ এসেছে Container Queries। এর মাধ্যমে কোনো একটি নির্দিষ্ট ডিভ বা প্যারেন্টের উইডথ অনুযায়ী তার ভেতরের এলিমেন্ট স্টাইল করা যায়।

CSS
/* প্যারেন্টকে কন্টেইনার হিসেবে ঘোষণা করা */
.widget-parent {
    container-type: inline-size;
}

/* কন্টেইনারের সাইজ ৪০০ পিক্সেলের বেশি হলে স্টাইল চেঞ্জ */
@container (min-width: 400px) {
    .widget-child {
        display: flex;
    }
}
Part 4: CSS Performance Engineering & Core Web Vitals
১. Layout Shifts (CLS) রোধ করা
ওয়েবসাইট লোড হওয়ার সময় ছবি বা ফন্টের কারণে কন্টেন্ট লাফিয়ে নিচে নেমে যাওয়াকে বলা হয় Cumulative Layout Shift (CLS)। এটি গুগলের একটি গুরুত্বপূর্ণ র‍্যাংকিং ফ্যাক্টর।

সমাধান: ইমেজে সবসময় নির্দিষ্ট width এবং height দিন অথবা সিএসএস-এ aspect-ratio প্রোপার্টি ব্যবহার করুন।

CSS
img {
    width: 100%;
    height: auto;
    aspect-ratio: 16 / 9; /* ইমেজ লোড হওয়ার আগেই ব্রাউজার জায়গা ধরে রাখবে */
}
২. Dynamic Viewport Units (dvh, dvw)
মোবাইল ব্রাউজারে (যেমন সাফারী বা ক্রোম) স্ক্রল করার সময় টপ বা বটম বার হাইড হয়ে যায়, ফলে 100vh দিলে লেআউট অনেক সময় কেটে যায়। এই সমস্যা সমাধানের জন্য মডার্ন সিএসএস-এ এসেছে Dynamic Viewport Units (dvh)।

CSS
.hero-section {
    height: 100dvh; /* মোবাইল অ্যাড্রেস বার ওঠানামা করলেও এটি পারফেক্ট স্ক্রিন সাইজ মেইনটেইন করবে */
}
Part 5: Elite Frontend Interview Questions & Answers
Q1: What is a Block Formatting Context (BFC) and how does it prevent Margin Collapsing?
Answer: BFC হলো সিএসএস রেন্ডারিংয়ের এমন একটি স্বাধীন এরিয়া, যেখানে ভেতরের এলিমেন্টগুলো বাইরের এলিমেন্টের লেআউটকে প্রভাবিত করে না।
দুটি পাশাপাশি ব্লকের মার্জিন যখন একে অপরের ভেতর ঢুকে যায় (Margin Collapsing), তখন প্যারেন্ট এলিমেন্টে display: flow-root; অথবা overflow: hidden; দিলে একটি নতুন BFC তৈরি হয়, যা মার্জিন কোলাপ্স হওয়া সম্পূর্ণ বন্ধ করে দেয়।

Q2: Explain the difference between Reflow (Layout) and Repaint (Paint) in browser rendering. Which CSS properties trigger what?
Answer: * Reflow: যখন কোনো সিএসএস এর কারণে এলিমেন্টের জ্যামিতিক আকার বা পজিশন পরিবর্তন হয়, তখন ব্রাউজারকে পুরো লেআউট পুনরায় গণনা করতে হয়। যেমন: width, height, margin, top, left পরিবর্তন করলে রিফ্লো হয়। এটি পারফরম্যান্সের জন্য ক্ষতিকর।

Repaint: এলিমেন্টের পজিশন ঠিক রেখে শুধু লুক বা কালার চেঞ্জ হলে ব্রাউজার রি পেইন্ট করে। যেমন: color, background-color, visibility।

Performance Tip: স্মুথ অ্যানিমেশনের জন্য রিফ্লো/রিপেইন্ট এড়িয়ে transform: translate() এবং opacity ব্যবহার করা উচিত, কারণ এগুলো সরাসরি GPU (Graphics Processing Unit) হ্যান্ডেল করে।

Q3: What is the difference between display: none and visibility: hidden?
Answer:

display: none: এলিমেন্টটিকে DOM এবং লেআউট ট্রি থেকে সম্পূর্ণ রিমুভ করে দেয়। ব্রাউজার এর জন্য কোনো জায়গা বরাদ্দ রাখে না এবং এটি একটি Reflow ট্রিগার করে।

visibility: hidden: এলিমেন্টটি স্ক্রিনে অদৃশ্য হয়ে যায়, কিন্তু লেআউটে তার নিজস্ব জায়গা দখল করে রাখে। এটি শুধুমাত্র Repaint ট্রিগার করে। স্ক্রিন রিডারও এই এলিমেন্টটিকে রিড করতে পারে না।

Q4: How does the browser calculate em versus rem units for font-size and padding?
Answer:

rem (Root EM): এটি সরাসরি রুট এলিমেন্টের (<html> ট্যাগের) ফন্ট-সাইজের ওপর ভিত্তি করে কাজ করে। ব্রাউজারের ডিফল্ট রুট সাইজ 16px হলে, 2rem = 32px।

em: এটি তার নিকটবর্তী প্যারেন্ট এলিমেন্টের (Font-size এর ক্ষেত্রে) অথবা নিজস্ব এলিমেন্টের ফন্ট সাইজের (Padding/Margin এর ক্ষেত্রে) ওপর নির্ভর করে রিলেটিভ স্কেলিং তৈরি করে।

Best Practice: গ্লোবাল স্কেলিং ও ফন্ট সাইজের জন্য rem এবং কম্পোনেন্টের ইন্টারনাল স্পেসিফিক প্যাডিং-মার্জিনের জন্য em ব্যবহার করা প্রফেশনাল স্ট্যান্ডার্ড।

Q5: What are Pseudo-classes and Pseudo-elements? Give a real-world architectural example.
Answer:

Pseudo-class (:): এলিমেন্টের বিশেষ একটি স্টেট (State) বোঝাতে ব্যবহৃত হয়। যেমন: :hover, :focus, :nth-child(2)।

Pseudo-element (::): এলিমেন্টের ভেতরের নির্দিষ্ট কোনো অংশকে স্টাইল করতে ব্যবহৃত হয়। যেমন: ::before, ::after, ::placeholder।

Real-world Example: ইনপুট বক্সের নিচে কাস্টম আন্ডারলাইন অ্যানিমেশন বা কোনো আইকন ডাইনামিক্যালি যুক্ত করতে আমরা ::after এর সাথে content: "" ব্যবহার করি, যাতে এক্সট্রা HTML ট্যাগ লিখতে না হয়।



Part 6: Enterprise-Level CSS Tooling & Preprocessors
১. CSS Preprocessors (Sass/SCSS)
বড় প্রজেক্টে হাজার হাজার লাইনের ভ্যানিলা সিএসএস মেইনটেইন করা দুঃস্বপ্ন। প্রফেশনালরা Sass (Syntactically Awesome Style Sheets) ব্যবহার করেন। এটি সিএসএস-কে একটি প্রোগ্রামিং ল্যাঙ্গুয়েজের মতো শক্তি দেয়।

Nesting: সিএসএস এর ভেতরে সিএসএস লেখা, যা BEM মেথডোলজির সাথে খুব ভালো কাজ করে।

Mixins: বারবার ব্যবহার করা কোড ব্লকগুলোকে ফাংশনের মতো সেভ করে রাখা।

SCSS
/* SCSS Example */
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  background: #fff;
  
  &__button {      /* এটি কম্পাইল হয়ে .card__button হবে */
    @include flex-center; /* মিক্সিন কল করা হলো */
    color: blue;
    
    &:hover {      /* হোভার স্টেট */
      color: red;
    }
  }
}
২. Utility-First Frameworks (Tailwind CSS) বনাম Custom CSS
ইন্ডাস্ট্রিতে এখন কাস্টম সিএসএস ফাইলের চেয়ে Tailwind CSS এর মতো ইউটিলিটি-ফার্স্ট ফ্রেমওয়ার্কের চাহিদা সবচেয়ে বেশি।

কেন শিখবেন: আপনি যখন প্রফেশনালি কম্পোনেন্ট বেসড (যেমন React বা Next.js) কাজ করবেন, তখন আলাদা সিএসএস ফাইল না বানিয়ে সরাসরি HTML এর ভেতরে ক্লাস নেম দিয়ে ডিজাইন করাটা ডেভেলপমেন্ট স্পিড ১০ গুণ বাড়িয়ে দেয়। এটি বান্ডেল সাইজও একদম ছোট রাখে।

Part 7: Advanced Media & Web Performance
১. Responsive Images & Art Direction (<picture> vs srcset)
প্রফেশনাল সাইটে মোবাইলের জন্য ছোট রেজোলিউশনের ছবি এবং ডেস্কটপের জন্য ৪কে (4K) ছবি লোড করানো হয়। একই ছবি সব ডিভাইসে লোড করলে সাইট স্লো হয়ে যায়।

srcset (Resolution Switching): ব্রাউজার ডিসপ্লের ডেনসিটি অনুযায়ী বেস্ট ছবিটি বেছে নেয়।

<picture> (Art Direction): মোবাইলে স্কয়ার (Square) ছবি আর ডেস্কটপে ওয়াইড (Wide) ছবি দেখাতে ব্যবহৃত হয়।

HTML
<!-- Art Direction Example -->
<picture>
  <!-- স্ক্রিন ৬০০px এর বড় হলে এই ছবি লোড হবে -->
  <source media="(min-width: 600px)" srcset="dashboard-desktop.webp">
  <!-- ডিফল্ট বা মোবাইলের জন্য এই ছবি লোড হবে -->
  <img src="dashboard-mobile.webp" alt="SyncDash Dashboard" loading="lazy">
</picture>
বিঃদ্রঃ loading="lazy" ব্যবহার করা প্রো-লেভেলের একটি সিম্পল ট্রিক, যা ইউজারের স্ক্রিনে ছবি আসার ঠিক আগ মুহূর্তে ছবি লোড করে ডেটা বাঁচায়।

Part 8: Advanced UI Animations & Micro-interactions
১. Hardware Acceleration (60FPS Animations)
ওয়েবসাইটে অ্যানিমেশন দিলে অনেক সময় তা আটকে আটকে (Laggy) যায়। প্রফেশনালরা জানেন যে ব্রাউজারের মেইন থ্রেড (CPU) দিয়ে অ্যানিমেশন না করিয়ে সেটি গ্রাফিক্স কার্ড (GPU) দিয়ে করাতে হয়।

সঠিক নিয়ম: অ্যানিমেশনের জন্য শুধু transform এবং opacity ব্যবহার করুন।

CSS
/* GPU কে সিগন্যাল দেওয়া যে এই এলিমেন্টটি চেঞ্জ হবে */
.modal {
  will-change: transform, opacity;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

/* 3D ট্রান্সফর্ম ব্যবহার করলে ব্রাউজার স্বয়ংক্রিয়ভাবে Hardware Acceleration অন করে দেয় */
.modal.open {
  transform: translate3d(0, 0, 0); 
  opacity: 1;
}
Part 9: CSS Architecture in Production
১. PostCSS & Autoprefixer
আপনি যে আধুনিক সিএসএস (যেমন Grid, Container Queries) লিখছেন, তা হয়তো পুরোনো ব্রাউজারে সাপোর্ট করবে না। প্রফেশনাল লেভেলে PostCSS এবং Autoprefixer এর মতো টুল ব্যবহার করা হয়, যা আপনার কোডকে স্বয়ংক্রিয়ভাবে পুরোনো ব্রাউজারের উপযোগী করে দেয় (Vendor prefixes যোগ করে)।

CSS
/* আপনি লিখবেন: */
.box {
  display: flex;
}

/* Autoprefixer প্রোডাকশনে অটোমেটিক বানিয়ে দেবে: */
.box {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
}
💼 Elite Technical Interview Questions (Senior Level)
প্রশ্ন ৬: Sass-এ @mixin এবং @extend এর মধ্যে মূল পার্থক্য কী এবং কখন কোনটি ব্যবহার করবেন?
উত্তর:

@mixin: এটি প্যারামিটার গ্রহণ করতে পারে (ফাংশনের মতো)। যখন একই স্টাইল বিভিন্ন ভ্যালু দিয়ে বারবার ব্যবহার করতে হয়, তখন এটি সেরা। তবে এটি প্রতিবার কম্পাইল হওয়ার সময় নতুন করে কোড তৈরি করে, ফলে সিএসএস ফাইলের সাইজ কিছুটা বড় হতে পারে।

@extend: এটি একাধিক ক্লাসকে একই সিএসএস রুলের আওতায় নিয়ে আসে (Grouping selectors)। এটি ফাইলের সাইজ ছোট রাখে, তবে মিডিয়া কোয়েরির ভেতরে ঠিকমতো কাজ করে না এবং স্পেসিফিসিটি চেইন জটিল করে ফেলতে পারে।

ইন্ডাস্ট্রি রুল: সেফ থাকার জন্য প্রফেশনালরা বেশিরভাগ সময় Mixin ব্যবহার করতে স্বাচ্ছন্দ্যবোধ করেন।

প্রশ্ন ৭: "Z-index" কাজ করছে না—এমন পরিস্থিতিতে আপনি কীভাবে ডিবাগ করবেন?
উত্তর: z-index শুধুমাত্র পজিশনড এলিমেন্টের (অর্থাৎ position: relative, absolute, fixed, বা sticky) ওপর কাজ করে। যদি পজিশন দেওয়া থাকার পরও কাজ না করে, তবে বুঝতে হবে এলিমেন্টটি অন্য একটি Stacking Context এর ভেতরে আটকে আছে। প্যারেন্ট এলিমেন্টের নিজস্ব কোনো z-index বা opacity থাকলে সেটি একটি নতুন স্ট্যাকিং কনটেক্সট তৈরি করে, যা চাইল্ড এলিমেন্টকে গ্লোবালি ওপরে উঠতে দেয় না। সমাধান হলো প্যারেন্টের স্ট্যাকিং কনটেক্সট ফিক্স করা।

প্রশ্ন ৮: Accessibility (A11y) এর ক্ষেত্রে "Focus Trap" কী এবং এটি ড্যাশবোর্ডের মোডাল (Modal) এর জন্য কেন গুরুত্বপূর্ণ?
উত্তর: যখন একটি পপ-আপ বা মোডাল ওপেন হয়, তখন কীবোর্ডের Tab বাটন চাপলে ফোকাস যেন মোডালের ভেতরেই আটকে থাকে এবং পেছনের (Background) কোনো এলিমেন্টে না চলে যায়, তাকেই ফোকাস ট্র্যাপ (Focus Trap) বলে। এটি জাভাস্ক্রিপ্ট দিয়ে কন্ট্রোল করতে হয়। প্রফেশনাল ড্যাশবোর্ডে অ্যাক্সেসিবিলিটি স্ট্যান্ডার্ড (WCAG) মেইনটেইন করার জন্য এটি বাধ্যতামূলক।