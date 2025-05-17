<script>
import { onMount } from 'svelte';
import { fade, fly, slide } from 'svelte/transition';
import '../app.css';
import emailjs from '@emailjs/browser';


let EMAILJS_SERVICE_ID = import.meta.env.VITE_EMAILJS_SERVICE_ID;
let EMAILJS_TEMPLATE_ID = import.meta.env.VITE_EMAILJS_TEMPLATE_ID;
let EMAILJS_PUBLIC_KEY = import.meta.env.VITE_EMAILJS_PUBLIC_KEY;

let texts = [
    "intelligent",
    "scalable",
    "reliable",
    "efficient",
  ];

let period = 2000;

let text = '';
let loopNum = 0;
let isDeleting = false;

const tick = () => {
    let i = loopNum % texts.length;
    let fullTxt = texts[i];

    if (isDeleting) {
      text = fullTxt.substring(0, text.length - 1);
    } else {
      text = fullTxt.substring(0, text.length + 1);
    }

    let delta = 200 - Math.random() * 100;
    if (isDeleting) delta /= 2;

    if (!isDeleting && text === fullTxt) {
      delta = period;
      isDeleting = true;
    } else if (isDeleting && text === '') {
      isDeleting = false;
      loopNum++;
      delta = 500;
    }

    setTimeout(tick, delta);
  }

const navItems = [
    { label: 'Home', href: '#home' },
    { label: 'Skills', href: '#skills' },
    { label: 'Experience', href: '#experience' },
    { label: 'Projects', href: '#projects' },
    { label: 'Education', href: '#education' },
    //{ label: 'Blog', href: '#blog' },
    { label: 'Contact', href: '#contact' }
];

const projects = [
    {
    title: 'Bio Insight Pro',
    description: 'AI-powered tool designed to assist doctors and researchers by analyzing large volumes of medical and research documents.',
    technologies: ['Django', 'Langchain', 'OpenAI', 'React', 'Pinecone'],
    image: '/bip.png',
    link: 'https://bioinsight.pro'
    },
    {
    title: 'CyberpscyhAI',
    description: 'AI Agent powered integrated X (formerly ~Twitter) bot which tweets interesting stuff.',
    technologies: ['Flask', 'Docker', 'Twitter API'],
    image: '/cyberpsychai.png?height=300&width=500',
    link: 'https://x.com/CyberpsychAI'
    }
];

const education = [
  {
    org:'Shri Ramdeobaba College of Engineering and Management',
    degree:'BTech. Computer Science and Engineering (Data Science)',
    period:'2022 - Present',
    grade:'9.08/10',
  }
]
const experiences = [
    {
    logo: 'IITB_Logo.svg',
    company: 'IIT Bombay',
    position: 'Research Intern',
    period: 'October 2024 - Present',
    description: 'Engineered a backend system for "Predict Xplore Tool" from scratch, enabling seamless user registration and secure OTP authentication. Incorporated multiple ML/DL like YOLO and etc. inference support into the software.'
    }
];

/*
const blogPosts = [
    {
    title: 'Sample Blog Post 1',
    excerpt: 'Learn how to create scalable and secure APIs using Django REST Framework with best practices for authentication and performance.',
    date: 'April 2, 2025',
    image: '/placeholder.svg?height=200&width=300',
    link: '/blog/django-rest-framework'
    } 
];
*/

let github_user = "cyberpsychofc"

let django_tiles = [
    "RedisSocketChat",
    "LocomotiveDataAPI"
    
  ];

let spring_tiles = [
    "Spring-Guides-Apps",
    "logauth",
    "Mini-Search-Engine"
];

let lg_tiles = [
  "Content-Engine-Langchain",
  "DocAnalyzerAI"
];

// Track the index of the tile being hovered over.
let hoveredIndex = -1;
let hoveredIndex2 = -1;
let hoveredIndex3 = -1;

let mobileMenuOpen = false;

let name = '';
let email = '';
let message = '';
let formSubmitted = false;
let formEmpty = false;
let formError = false;
let form;
let isSpam = false;

let darkMode = false;

onMount(() => {
    if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
    darkMode = true;
    document.documentElement.classList.add('dark');
    }

    tick();

    const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
        activeSection = entry.target.id;
        entry.target.classList.add('in-view');
        }
    });
    }, { threshold: 0.2 });

    sections = document.querySelectorAll('section');
    sections.forEach(section => observer.observe(section));

    return () => {
    sections.forEach(section => observer.unobserve(section));
    };
});

const handleSubmit = async () => {
    if (!name.trim() || !email.trim() || !message.trim()) {
      formEmpty = true;
      return;
    }
    if (email=='aryan2003k@gmail.com'){
      isSpam = true;
      return;
    }

    try {
      const result = await emailjs.sendForm(
        EMAILJS_SERVICE_ID,
        EMAILJS_TEMPLATE_ID,
        form,
        EMAILJS_PUBLIC_KEY
      );
      formError = false;
      formEmpty = false;
      isSpam = false;

      name = '';
      email = '';
      message = '';
      formSubmitted = true;
    } catch (error) {
      formError = true;
      formEmpty = false;
    }
  };

const toggleDarkMode = () => {
    darkMode = !darkMode;
    if (darkMode) {
    document.documentElement.classList.add('dark');
    } else {
    document.documentElement.classList.remove('dark');
    }
};

const downloadResume = async () => {
  const response = await fetch('/resume.pdf');
  const blob = await response.blob();

  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = 'Om_Aryan_Resume.pdf'; // filename forced
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
};

let sections = [];
let activeSection = 'home';
</script>

<svelte:head>
  <title>Om Aryan</title>
  <meta name="description" content="Portfolio of Om Aryan, a backend developer specializing in building solutions." />
  <meta name="keywords" content="backend developer, Django, Langchain, SpringBoot, deep learning, ai" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@700;800&display=swap" rel="stylesheet">
</svelte:head>

<div class="bg-gradient-to-br from-gray-50 to-gray-100 dark:from-gray-900 dark:to-gray-950 text-gray-900 dark:text-gray-100 min-h-screen font-sans transition-colors duration-300">
    <!-- Background decorative elements -->
    <div class="fixed inset-0 overflow-hidden pointer-events-none z-0">
      <div class="absolute top-0 left-0 w-full h-full opacity-30 dark:opacity-10">
        <div class="absolute top-20 left-10 w-64 h-64 rounded-full bg-primary/10 blur-3xl"></div>
        <div class="absolute bottom-20 right-10 w-80 h-80 rounded-full bg-emerald-500/10 blur-3xl"></div>
        <div class="absolute top-1/3 right-1/4 w-72 h-72 rounded-full bg-amber-500/10 blur-3xl"></div>
      </div>
    </div>
  
    <!-- Header -->
    <header class="fixed w-full bg-white/80 dark:bg-gray-900/80 backdrop-blur-md shadow-sm z-50 transition-all duration-300">
      <div class="container mx-auto px-4 py-3 flex justify-between items-center">
        <a href="#home" class="text-2xl font-mono font-bold text-primary dark:text-primary-foreground relative group flex items-center">
          <img src="/favicon.png" alt="Logo" class="w-8 h-8 mr-2" />
          <!-- <span class="absolute bottom-0 left-0 w-full h-3 bg-primary/10 dark:bg-primary/20 rounded-lg -z-0 group-hover:h-4 transition-all duration-300"></span>-->
        </a>
        
        <!-- Desktop Navigation -->
        <nav class="hidden md:flex space-x-8">
          {#each navItems as item}
            <a 
              href={item.href} 
              class="text-gray-700 dark:text-gray-300 hover:text-primary dark:hover:text-primary-foreground transition-colors duration-200 relative py-2 {activeSection === item.href.substring(1) ? 'text-primary dark:text-primary-foreground font-medium' : ''}"
            >
              {item.label}
              {#if activeSection === item.href.substring(1)}
                <span class="absolute bottom-0 left-0 w-full h-0.5 bg-primary dark:bg-primary-foreground rounded-full"></span>
              {/if}
            </a>
          {/each}
        </nav>
        
        <!-- Theme toggle and mobile menu -->
        <div class="flex items-center gap-4">
          
          <button 
            class="md:hidden w-10 h-10 rounded-full bg-gray-100 dark:bg-gray-800 flex items-center justify-center hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors duration-200 focus:outline-none focus:ring-2 focus:ring-primary" 
            on:click={() => mobileMenuOpen = !mobileMenuOpen}
            aria-label="Toggle menu"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={mobileMenuOpen ? "M6 18L18 6M6 6l12 12" : "M4 6h16M4 12h16M4 18h16"} />
            </svg>
          </button>
        </div>
      </div>
      
      <!-- Mobile Navigation -->
      {#if mobileMenuOpen}
        <nav 
          class="md:hidden bg-white dark:bg-gray-900 py-4 px-4 shadow-lg"
          transition:slide={{ duration: 200 }}
        >
          {#each navItems as item}
            <a 
              href={item.href} 
              class="block py-3 px-4 my-1 rounded-lg text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 hover:text-primary dark:hover:text-primary-foreground transition-colors duration-200 {activeSection === item.href.substring(1) ? 'bg-primary/5 dark:bg-primary/10 text-primary dark:text-primary-foreground font-medium' : ''}"
              on:click={() => mobileMenuOpen = false}
            >
              {item.label}
            </a>
          {/each}
        </nav>
      {/if}
    </header>
  
    <!-- Hero Section -->
    <section id="home" class="pt-32 pb-20 md:pl-10 md:pt-40 md:pb-32 relative overflow-hidden">
      <div class="container mx-auto px-6 flex flex-col md:flex-row items-center relative z-10">
        <div class="md:w-1/2 mb-12 md:mb-0">
          <div class="inline-block px-4 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-6">
            Software Developer
          </div>
          <h1 class="text-4xl md:text-5xl lg:text-6xl font-mono font-bold mb-6 leading-tight">
            Building <br /><span class="text-primary dark:text-primary-foreground relative"><span class="wrap">{text}</span><span class="absolute bottom-2 left-0 w-full h-3 bg-primary/20 dark:bg-primary/30 -z-10 rounded-lg "></span><br /></span> systems for the modern age
          </h1>
          <p class="text-2xl mb-2 text-black dark:text-white max-w-lg leading-relaxed">
            Hi! I'm <b class="highlight text-shadow-2xs">Om Aryan</b></p>
          <p class="text-base mb-8 bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-400 dark:bg-clip-text dark:text-transparent leading-relaxed pt-3">
            A Passionate programmer with a love for challenges and problem-solving in building solutions. Skilled in Java and Python always eager to expand my knowledge and skills in Deep Learning and Software Development.
          </p>
          <div class="flex flex-wrap gap-4">
            <a 
              href="#contact" 
              class="px-8 py-3 bg-primary text-white rounded-full hover:bg-primary/90 transition-all duration-300 shadow-md hover:shadow-lg hover:translate-y-[-2px] font-medium"
            >
              Get in Touch
            </a>
            <a 
              href="#projects" 
              class="px-8 py-3 border-2 border-primary text-primary dark:text-primary-foreground rounded-full hover:bg-primary/10 transition-all duration-300 font-medium"
            >
              View Projects
            </a>
          </div>
        </div>
        <div class="md:w-1/2 flex justify-center">
          <div class="relative">
            <div class="absolute inset-0 bg-gradient-to-br from-primary/20 to-emerald-500/20 rounded-full blur-3xl"></div>
            <div class="gradient-border relative rounded-full w-64 h-64 md:w-80 md:h-80 shadow-2xl">
              <div class="overflow-hidden rounded-full w-full h-full">
                <img
                  src="/sudo2.jpg?height=600&width=600"
                  alt="Om Aryan"
                  class="w-full h-full object-cover"
                />
              </div>
            </div>
            <!--
            <div class="absolute -bottom-4 -right-4 w-24 h-24 bg-white dark:bg-gray-800 rounded-full flex items-center justify-center shadow-lg">
              <div class="text-center">
                <div class="text-2xl font-bold text-primary">5+</div>
                <div class="text-xs text-gray-600 dark:text-gray-400">Years Exp.</div>
              </div>
            </div>-->
          </div>
        </div>
      </div>
    </section>
  
    <!-- Skills Section -->
    <section id="skills" class="py-20 relative">
      <div class="absolute inset-0 bg-gradient-to-b from-gray-100 to-white dark:from-gray-900 dark:to-gray-950 -z-10"></div>
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Technical Expertise
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">Skills & Technologies</h2>
          <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-400 dark:bg-clip-text dark:text-transparent leading-relaxed">
            As I continue learning, I work with backend technologies and AI frameworks to build scalable and intelligent systems that tackle complex problems.
          </p>
        </div>
        
        <div class="mt-20 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 md:pr-10 md:pl-10">
          <div class="bg-white/50 dark:bg-gray-800/50 backdrop-blur-sm rounded-2xl p-6 border border-gray-200 dark:border-gray-700 transition-all duration-400 hover:translate-y-[-7px]">
            <h4 class="text-lg font-bold mb-3 flex items-center">
              <span class="w-8 h-8 rounded-full bg-primary/10 flex items-center justify-center mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" />
                </svg>
              </span>
              Languages
            </h4>
            <div class="ml-11 flex flex-wrap gap-2">
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Java</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Python</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">C</span>
                <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">SQL</span>
              
            </div>
          </div>
          <div class="bg-white/50 dark:bg-gray-800/50 backdrop-blur-sm rounded-2xl p-6 border border-gray-200 dark:border-gray-700 transition-all duration-400 hover:translate-y-[-7px]">
            <h4 class="text-lg font-bold mb-3 flex items-center">
              <span class="w-8 h-8 rounded-full bg-primary/10 flex items-center justify-center mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4" />
                </svg>
              </span>
              Databases
            </h4>
            <div class="ml-11 flex flex-wrap gap-2">
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">MySQL</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">OracleDB</span>              
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Pinecone</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Redis</span>
            </div>
          </div>
          <div class="bg-white/50 dark:bg-gray-800/50 backdrop-blur-sm rounded-2xl p-6 border border-gray-200 dark:border-gray-700 transition-all duration-400 hover:translate-y-[-7px]">
            <h4 class="text-lg font-bold mb-3 flex items-center">
              <span class="w-8 h-8 rounded-full bg-primary/10 flex items-center justify-center mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 4a2 2 0 114 0v1a1 1 0 001 1h3a1 1 0 011 1v3a1 1 0 01-1 1h-1a2 2 0 100 4h1a1 1 0 011 1v3a1 1 0 01-1 1h-3a1 1 0 01-1-1v-1a2 2 0 10-4 0v1a1 1 0 01-1 1H7a1 1 0 01-1-1v-3a1 1 0 00-1-1H4a2 2 0 110-4h1a1 1 0 001-1V7a1 1 0 011-1h3a1 1 0 001-1V4z" />
                </svg>
              </span>
              Deployment
            </h4>
            <div class="ml-11 flex flex-wrap gap-2">
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Docker</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Git</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Amazon Web Services</span>
            </div>
          </div>
          <div class="bg-white/50 dark:bg-gray-800/50 backdrop-blur-sm rounded-2xl p-6 border border-gray-200 dark:border-gray-700 transition-all duration-400 hover:translate-y-[-7px]">
            <h4 class="text-lg font-bold mb-3 flex items-center">
              <span class="w-8 h-8 rounded-full bg-primary/10 flex items-center justify-center mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z" />
                </svg>
              </span>
              Web Frameworks
            </h4>
            <div class="ml-11 flex flex-wrap gap-2">
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Django</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Spring Boot</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">FastAPI</span>
              <span class="px-3 py-1 bg-gray-100 dark:bg-gray-700 rounded-full text-sm">Flask</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Experience Section -->
    <section id="experience" class="py-20 bg-gradient-to-b from-gray-50 to-white dark:from-gray-900 dark:to-gray-950">
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Professional Journey
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">Work Experience</h2>
          <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-400 dark:bg-clip-text dark:text-transparent leading-relaxed">
            A track record of success in developing innovative solutions across various industries and technologies.
          </p>
        </div>
        
        <div class="max-w-4xl mx-auto">
          {#each experiences as experience, i}
            <div 
              class="relative pl-10 pb-16 {i !== experiences.length - 1 ? 'border-l-2 border-primary/20' : ''}"
              in:fly={{ y: 20, delay: i * 150, duration: 500 }}
            >
              <div class="absolute left-[-10px] top-0 w-5 h-5 rounded-full bg-primary border-4 border-white dark:border-gray-900"></div>
              <div class="mb-4">
                <span class="inline-block px-4 py-2 bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium rounded-full mb-2">
                  {experience.period}
                </span>
              </div>
              <div class="bg-white dark:bg-gray-800 rounded-2xl p-6 shadow-lg hover:shadow-xl transition-all duration-300 border border-gray-100 dark:border-gray-700 hover:translate-y-[-5px]">
                <h3 class="text-xl font-bold mb-1 text-shadow-2xs dark:text-shadow-2xs">{experience.position}</h3>
                <div class="flex items-center mb-4">
                  <img src="{experience.logo}" alt="{experience.company} Logo" class="w-8 h-8 mr-3 rounded-full" />
                  <h4 class="text-lg text-gray-600 dark:text-gray-300 text-shadow-2xs dark:text-shadow-2xs  ">{experience.company}</h4>
                </div>
                <p class="text-gray-600 dark:text-gray-400 leading-relaxed text-shadow-2xs dark:text-shadow-2xs">{experience.description}</p>
              </div>
            </div>
          {/each}
        </div>
        
        <div class="mt-16 text-center">
            <button 
            on:click={downloadResume} 
            class="inline-flex items-center px-8 py-3 bg-white dark:bg-gray-800 border-2 border-primary text-primary dark:text-primary-foreground rounded-full hover:bg-primary/5 transition-all duration-300 font-medium group shadow-lg hover:shadow-xl active:scale-95"
            >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
            </svg>
            Download Resume
            </button>
        </div>
      </div>
    </section>
  
    <!-- Projects Section -->
    <section id="projects" class="py-20 bg-gradient-to-b from-white to-gray-50 dark:from-gray-950 dark:to-gray-900">
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Featured Work
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">Projects in Action</h2>
          <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-400 dark:bg-clip-text dark:text-transparent leading-relaxed">
            Explore a selection of my most significant projects, showcasing my expertise in backend development and AI solutions.
          </p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-10 md:pr-10 md:pl-10">
          {#each projects as project, i}
            <div 
              class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl overflow-hidden group hover:shadow-2xl transition-all duration-300 hover:translate-y-[-5px]"
              in:fly={{ y: 20, delay: i * 150, duration: 500 }}
            >
              <div class="relative overflow-hidden">
                <img 
                  src={project.image} 
                  alt={project.title} 
                  class="w-full h-56 object-cover object-left group-hover:scale-105 transition-transform duration-500"
                />
                <div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end">
                  <div class="p-6">
                    <h3 class="text-xl font-bold text-white mb-2">{project.title}</h3>
                    <p class="text-gray-200 text-sm">{project.description}</p>
                  </div>
                </div>
              </div>
              <div class="p-6">
                <h3 class="text-xl font-bold mb-3 group-hover:text-primary dark:group-hover:text-primary-foreground transition-colors duration-300">{project.title}</h3>
                <div class="flex flex-wrap gap-2 mb-6">
                  {#each project.technologies as tech}
                    <span class="px-3 py-1 bg-primary/10 text-primary dark:text-primary-foreground text-sm rounded-full">
                      {tech}
                    </span>
                  {/each}
                </div>
                <a 
                  href={project.link} 
                  class="inline-flex items-center text-primary dark:text-primary-foreground font-medium hover:underline group/link"
                >
                  View Project
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2 group-hover/link:translate-x-1 transition-transform duration-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                  </svg>
                </a>
              </div>
            </div>
          {/each}
        </div>
        
        <div class="mt-16 text-center">
          <a 
            href="https://github.com/cyberpsychofc?tab=repositories" 
            class="inline-flex items-center px-8 py-3 border-2 border-primary text-primary dark:text-primary-foreground rounded-full hover:bg-primary/10 transition-all duration-300 font-medium group"
          >
            View All Projects
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2 group-hover:translate-x-1 transition-transform duration-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
            </svg>
          </a>
        </div>
      </div>
    </section>
  
    <!-- Technology Sections -->
    <section id="projects" class="py-20">
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Core Technologies
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">What I Love Working With</h2>
          <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-400 dark:bg-clip-text dark:text-transparent leading-relaxed">
            My experience and hands-on experience with technologies and frameworks for building robust and intelligent softwares.
          </p>
        </div>
        
        <div class="space-y-24">
          <!-- Django Section -->
          <div class="grid grid-cols-1 md:pr-10 lg:grid-cols-2 gap-12 items-center">
            <div class="order-2">
              <div class="flex items-center mb-6">
                <div class="w-14 h-14 bg-gradient-to-br from-green-500/20 to-green-500/5 dark:from-green-500/30 dark:to-green-500/10 rounded-2xl flex items-center justify-center mr-4">
                  <img src="/django.svg?height=100&width=100" alt="Django" class="w-8 h-8" />
                </div>
                <h3 class="text-2xl font-bold">Django</h3>
              </div>
              
              <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-200 dark:bg-clip-text dark:text-transparent mb-8 leading-relaxed">
                I love building scalable and robust web applications using Django. Its clean design, powerful ORM, and built-in admin make it my go-to framework for quickly turning ideas into production-ready systems. From developing APIs to managing databases and user authentication, I enjoy working with every part of the Django ecosystem.
              </p>
              
              
            </div>
            
            <div class="order-1 flex justify-center">
              <div class="relative">
                <div class="absolute inset-0 bg-gradient-to-br from-green-500/20 to-primary/20 rounded-2xl blur-3xl"></div>
                <div class="relative dark:bg-[#1e293b] text-gray-900 dark:text-gray-200 rounded-2xl shadow-2xl p-0 md:p-5 border border-gray-100 dark:border-gray-700 w-[300px] md:w-[400px]">
                  <div class="drawer">
                    {#each django_tiles as repo, index}
                      <div 
                        class="tile {hoveredIndex === index ? 'hovered' : ''} {hoveredIndex >= 0 && index > hoveredIndex ? 'shifted' : ''}"
                        on:mouseenter={() => hoveredIndex = index}
                        on:mouseleave={() => hoveredIndex = -1}
                      >
                      <a href="https://github.com/{github_user}/{repo}" target="_blank" rel="noopener">
                        <img src="https://gh-card.dev/repos/{github_user}/{repo}.svg"
                        class="dark:invert rounded-2xl" 
                        alt="GitHub Card" 
                        />
                      </a>
                      </div>
                    {/each}
                  </div>
                </div>
                <div class="absolute -bottom-6 -right-6 bg-white dark:bg-gray-800 rounded-full p-4 shadow-lg border border-gray-100 dark:border-gray-700 z-10">
                  <img src="/favicon.png?height=60&width=60" alt="Cyberpsych Logo" class="w-12 h-12" />
                </div>
              </div>
            </div>
          </div>
          
          <!-- Spring Section -->
          <div class="grid md:pl-10 grid-cols-1 lg:grid-cols-2 gap-12 items-center">
            <div class="order-2 lg:order-1">
              <div class="flex items-center mb-6">
                <div class="w-14 h-14 bg-gradient-to-br from-green-300/20 to-green-300/5 dark:from-green-200/30 dark:to-green-200/10 rounded-2xl flex items-center justify-center mr-4">
                  <img src="/spring.svg?height=100&width=100" alt="Spring Boot" class="w-8 h-8" />
                </div>
                <h3 class="text-2xl font-bold">Spring Boot</h3>
              </div>
              
              <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-200 dark:bg-clip-text dark:text-transparent mb-8 leading-relaxed">
                I’m actively learning to build scalable, enterprise-grade applications with Spring Boot. Its structured approach, rich ecosystem, and seamless integration capabilities make it a strong foundation for backend development. I value the opportunity to deepen my skills through hands-on experience with real-world projects.
              </p>
              
              
            </div>
            
            <div class="order-1 lg:order-2 flex justify-center">
              <div class="relative">
                <div class="absolute inset-0 bg-gradient-to-br from-green-500/20 to-primary/20 rounded-2xl blur-3xl"></div>
                <div class="relative dark:bg-[#1e293b] text-gray-900 dark:text-gray-200 rounded-2xl shadow-2xl p-0 md:p-5 border border-gray-100 dark:border-gray-700 w-[300px] md:w-[400px]">
                  <div class="drawer">
                    {#each spring_tiles as repo, index}
                      <div 
                        class="tile {hoveredIndex2 === index ? 'hovered' : ''} {hoveredIndex2 >= 0 && index > hoveredIndex2 ? 'shifted' : ''}"
                        on:mouseenter={() => hoveredIndex2 = index}
                        on:mouseleave={() => hoveredIndex2 = -1}
                      >
                      <a href="https://github.com/{github_user}/{repo}" target="_blank" rel="noopener">
                        <img src="https://gh-card.dev/repos/{github_user}/{repo}.svg"
                        class="dark:invert rounded-2xl" 
                        alt="GitHub Card" 
                        />
                      </a>
                      </div>
                    {/each}
                  </div>
                </div>
                <div class="absolute -bottom-6 -left-6 bg-white dark:bg-gray-800 rounded-full p-4 shadow-lg border border-gray-100 dark:border-gray-700 z-10">
                  <img src="/favicon.png?height=60&width=60" alt="Cyberpsych Logo" class="w-12 h-12" />
                </div>
              </div>
            </div>
          </div>
          
          <!-- Langchain Section -->
          <div class="grid grid-cols-1 md:pr-10 lg:grid-cols-2 gap-12 items-center">
            <div class="order-2">
              <div class="flex items-center mb-6">
                <div class="w-14 h-14 bg-gradient-to-br from-amber-400/20 to-amber-400/10 dark:from-amber-400/30 dark:to-amber-400/20 rounded-2xl flex items-center justify-center mr-4">
                  <img src="/langchain.svg?height=100&width=100" alt="Langchain" class="w-8 h-8" />
                </div>
                <h3 class="text-2xl font-bold">Langchain</h3>
              </div>
              
              <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-yellow-100 dark:to-yellow-200 dark:bg-clip-text dark:text-transparent mb-8 leading-relaxed">
                I'm equally passionate about leveraging the power of Large Language Models to build sophisticated AI applications with Langchain's flexible framework for LLM orchestration. Whether it's chaining prompts, integrating with vector databases, or deploying document-based question answering systems, I enjoy pushing the limits of what’s possible with AI and automation.
              </p>
              
              
            </div>
            
            <div class="order-1 flex justify-center">
              <div class="relative">
                <div class="absolute inset-0 bg-gradient-to-br from-amber-500/20 to-primary/20 rounded-2xl blur-3xl"></div>
                <div class="relative dark:bg-[#1e293b] text-gray-900 dark:text-gray-200 rounded-2xl shadow-2xl p-0 md:p-5 border border-gray-100 dark:border-gray-700 w-[300px] md:w-[400px]">
                  <div class="drawer">
                    {#each lg_tiles as repo, index}
                      <div 
                        class="tile {hoveredIndex3 === index ? 'hovered' : ''} {hoveredIndex3 >= 0 && index > hoveredIndex3 ? 'shifted' : ''}"
                        on:mouseenter={() => hoveredIndex3 = index}
                        on:mouseleave={() => hoveredIndex3 = -1}
                      >
                      <a href="https://github.com/{github_user}/{repo}" target="_blank" rel="noopener">
                        <img src="https://gh-card.dev/repos/{github_user}/{repo}.svg"
                        class="dark:invert rounded-2xl" 
                        alt="GitHub Card" 
                        />
                      </a>
                      </div>
                    {/each}
                  </div>
                <div class="absolute -bottom-6 -right-6 bg-white dark:bg-gray-800 rounded-full p-4 shadow-lg border border-gray-100 dark:border-gray-700 z-10">
                  <img src="/favicon.png?height=60&width=60" alt="Cyberpsych Logo" class="w-12 h-12" />
                </div>
              </div>
            </div>
          </div>
          
          <!-- Other technology sections would follow the same pattern -->
        </div>
      </div>
    </section>
    <!-- Education Section -->
    <section id="education" class="py-20 bg-gradient-to-b from-gray-50 to-white dark:from-gray-900 dark:to-gray-950">
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Educational Journey
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">Education</h2>
        </div>
        
        <div class="max-w-4xl mx-auto">
          {#each education as edu, i}
            <div 
              class="relative pl-10 pb-16 {i !== education.length - 1 ? 'border-l-2 border-primary/20' : ''}"
              in:fly={{ y: 20, delay: i * 150, duration: 500 }}
            >
              <div class="absolute left-[-10px] top-0 w-5 h-5 rounded-full bg-primary border-4 border-white dark:border-gray-900"></div>
              <div class="mb-4">
                <span class="inline-block px-4 py-2 bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium rounded-full mb-2">
                  {edu.period}
                </span>
              </div>
              <div class="bg-white dark:bg-gray-800 rounded-2xl p-6 shadow-lg hover:shadow-xl transition-all duration-300 border border-gray-100 dark:border-gray-700 hover:translate-y-[-5px]">
                <h3 class="text-xl font-bold mb-1 text-shadow-2xs dark:text-shadow-2xs">{edu.org}</h3>
                <h4 class="text-lg text-gray-600 dark:text-gray-300 mb-4 text-shadow-2xs dark:text-shadow-2xs">{edu.degree}</h4>
                <p class="text-gray-600 dark:text-gray-400 leading-relaxed text-shadow-2xs dark:text-shadow-2xs">CGPA: <b>{edu.grade}</b></p>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </section>
  
    <!-- Blog Section
    <section id="blog" class="py-20 bg-gradient-to-b from-white to-gray-50 dark:from-gray-950 dark:to-gray-900">
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Knowledge Sharing
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">Latest Articles & Insights</h2>
          <p class="text-gray-600 dark:text-gray-400 leading-relaxed">
            Sharing expertise, best practices, and insights on backend development, AI, and emerging technologies.
          </p>
        </div>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          {#each blogPosts as post, i}
            <div 
              class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl overflow-hidden group hover:shadow-2xl transition-all duration-300 hover:translate-y-[-5px]"
              in:fly={{ y: 20, delay: i * 150, duration: 500 }}
            >
              <div class="relative overflow-hidden h-48">
                <img 
                  src={post.image || "/placeholder.svg"} 
                  alt={post.title} 
                  class="w-full h-full object-cover object-center group-hover:scale-105 transition-transform duration-500"
                />
                <div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end">
                  <div class="p-6">
                    <a 
                      href={post.link} 
                      class="inline-flex items-center text-white font-medium hover:underline"
                    >
                      Read Article
                      <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                      </svg>
                    </a>
                  </div>
                </div>
              </div>
              <div class="p-6">
                <div class="flex items-center mb-3">
                  <div class="text-sm text-gray-500 dark:text-gray-400">{post.date}</div>
                  <div class="ml-auto px-3 py-1 bg-primary/10 text-primary dark:text-primary-foreground text-xs rounded-full">
                    Tutorial
                  </div>
                </div>
                <h3 class="text-xl font-bold mb-3 group-hover:text-primary dark:group-hover:text-primary-foreground transition-colors duration-300">{post.title}</h3>
                <p class="text-gray-600 dark:text-gray-300 mb-4 line-clamp-3">{post.excerpt}</p>
                <a 
                  href={post.link} 
                  class="inline-flex items-center text-primary dark:text-primary-foreground font-medium hover:underline group/link"
                >
                  Read More
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2 group-hover/link:translate-x-1 transition-transform duration-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                  </svg>
                </a>
              </div>
            </div>
          {/each}
        </div>
        
        <div class="mt-16 text-center">
          <a 
            href="/blog" 
            class="inline-flex items-center px-8 py-3 border-2 border-primary text-primary dark:text-primary-foreground rounded-full hover:bg-primary/10 transition-all duration-300 font-medium group"
          >
            View All Articles
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-2 group-hover:translate-x-1 transition-transform duration-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
            </svg>
          </a>
        </div>
      </div>
    </section>
   -->
    <!-- Contact Section -->
    <section id="contact" class="py-20 relative">
      <div class="absolute inset-0 bg-gradient-to-b from-gray-50 to-white dark:from-gray-900 dark:to-gray-950 -z-10"></div>
      <div class="container mx-auto px-6">
        <div class="text-center max-w-3xl mx-auto mb-16">
          <div class="inline-block px-3 py-1 rounded-full bg-primary/10 text-primary dark:text-primary-foreground text-sm font-medium mb-4">
            Let's Connect
          </div>
          <h2 class="text-3xl md:text-4xl font-mono font-bold mb-6">Get In Touch</h2>
          <p class="bg-gradient-to-bl from-gray-400 to-gray-700 bg-clip-text text-transparent text-shadow-md dark:text-shadow-lg dark:bg-gradient-to-bl dark:from-green-100 dark:to-green-400 dark:bg-clip-text dark:text-transparent leading-relaxed">
            Have a project in mind or want to discuss potential opportunities? I'd love to hear from you.
          </p>
        </div>
        
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 max-w-5xl mx-auto">
          <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl p-8 border border-gray-100 dark:border-gray-700">
            <h3 class="text-2xl font-bold mb-6">Contact Information</h3>
            <p class="text-gray-600 dark:text-gray-300 mb-8 leading-relaxed">
              Feel free to reach out if you're looking for a developer, have a question, or just want to connect.
            </p>
            
            <div class="space-y-6">
              <div class="flex items-center">
                <div class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center mr-4">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary dark:text-primary-foreground" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                  </svg>
                </div>
                <div>
                  <div class="text-sm text-gray-500 dark:text-gray-400 mb-1">Email</div>
                  <a href="mailto:aryan2003k@gmail.com" class="text-lg font-medium hover:text-primary dark:hover:text-primary-foreground transition-colors duration-200">aryan2003k@gmail.com</a>
                </div>
              </div>
              
              <div class="flex items-center">
                <div class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center mr-4">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary dark:text-primary-foreground" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
                  </svg>
                </div>
                <div>
                  <div class="text-sm text-gray-500 dark:text-gray-400 mb-1">Phone</div>
                  <a href="tel:+1234567890" class="text-lg font-medium hover:text-primary dark:hover:text-primary-foreground transition-colors duration-200">+91 9892593262</a>
                </div>
              </div>
              
              <div class="flex items-center">
                <div class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center mr-4">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-primary dark:text-primary-foreground" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                  </svg>
                </div>
                <div>
                  <div class="text-sm text-gray-500 dark:text-gray-400 mb-1">Location</div>
                  <span class="text-lg font-medium">Navi Mumbai, Maharashtra, India</span>
                </div>
              </div>
            </div>
            
            <div class="mt-10">
              <h4 class="text-lg font-bold mb-4">Connect with me</h4>
              <div class="flex space-x-4">
                <a href="https://www.linkedin.com/in/omaryan" class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center hover:bg-primary/20 transition-colors duration-200 group">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="text-primary dark:text-primary-foreground group-hover:scale-110 transition-transform duration-200" viewBox="0 0 16 16">
                    <path d="M0 1.146C0 .513.526 0 1.175 0h13.65C15.474 0 16 .513 16 1.146v13.708c0 .633-.526 1.146-1.175 1.146H1.175C.526 16 0 15.487 0 14.854V1.146zm4.943 12.248V6.169H2.542v7.225h2.401zm-1.2-8.212c.837 0 1.358-.554 1.358-1.248-.015-.709-.52-1.248-1.342-1.248-.822 0-1.359.54-1.359 1.248 0 .694.521 1.248 1.327 1.248h.016zm4.908 8.212V9.359c0-.216.016-.432.08-.586.173-.431.568-.878 1.232-.878.869 0 1.216.662 1.216 1.634v3.865h2.401V9.25c0-2.22-1.184-3.252-2.764-3.252-1.274 0-1.845.7-2.165 1.193v.025h-.016a5.54 5.54 0 0 1 .016-.025V6.169h-2.4c.03.678 0 7.225 0 7.225h2.4z"/>
                  </svg>
                </a>
                <a href="https://github.com/cyberpsychofc" class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center hover:bg-primary/20 transition-colors duration-200 group">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="text-primary dark:text-primary-foreground group-hover:scale-110 transition-transform duration-200" viewBox="0 0 16 16">
                    <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z"/>
                  </svg>
                </a>
                <a href="https://x.com/cyberpsychofc" class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center hover:bg-primary/20 transition-colors duration-200 group">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="text-primary dark:text-primary-foreground group-hover:scale-110 transition-transform duration-200" viewBox="0 0 16 16">
                    <image href="/x.svg" width="18" height="18" />
                  </svg>
                </a>
                <a href="https://devfolio.co/@cyberpsych" class="w-12 h-12 rounded-full bg-primary/10 flex items-center justify-center hover:bg-primary/20 transition-colors duration-200 group">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="text-primary dark:text-primary-foreground group-hover:scale-110 transition-transform duration-200" viewBox="0 0 16 16">
                    <image href="/devf.svg" width="18" height="18" />
                  </svg>
                </a>
                
              </div>
            </div>
          </div>
          
          <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-xl p-8 border border-gray-100 dark:border-gray-700">
            <h3 class="text-2xl font-bold mb-6">Send Me a Message</h3>
            
            {#if formSubmitted}
              <div class="bg-green-50 dark:bg-green-900/30 text-green-800 dark:text-green-300 p-6 rounded-xl mb-6 border border-green-200 dark:border-green-800" transition:fade>
                <div class="flex items-center mb-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-green-600 dark:text-green-400 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                  </svg>
                  <h4 class="text-lg font-bold text-green-800 dark:text-green-300">Message Sent!</h4>
                </div>
                <p class="text-green-700 dark:text-green-400">Thank you for your message! I'll get back to you as soon as possible.</p>
              </div>
            {/if}
            
            {#if formError}
              <div class="bg-red-50 dark:bg-red-900/30 text-red-800 dark:text-red-300 p-6 rounded-xl mb-6 border border-red-200 dark:border-red-800" transition:fade>
                <div class="flex items-center mb-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-600 dark:text-red-400 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <h4 class="text-lg font-bold text-red-800 dark:text-red-300">Error</h4>
                </div>
                <p class="text-red-700 dark:text-red-400">Message was not sent, please try again later!</p>
              </div>
            {/if}

            {#if isSpam}
              <div class="bg-red-50 dark:bg-red-900/30 text-red-800 dark:text-red-300 p-6 rounded-xl mb-6 border border-red-200 dark:border-red-800" transition:fade>
                <div class="flex items-center mb-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-600 dark:text-red-400 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <h4 class="text-lg font-bold text-red-800 dark:text-red-300">Possible Spam</h4>
                </div>
                <p class="text-red-700 dark:text-red-400">Please provide only your own accurate information.</p>
              </div>
            {/if}

            {#if formEmpty}
              <div class="bg-red-50 dark:bg-red-900/30 text-red-800 dark:text-red-300 p-6 rounded-xl mb-6 border border-red-200 dark:border-red-800" transition:fade>
                <div class="flex items-center mb-2">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-600 dark:text-red-400 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <h4 class="text-lg font-bold text-red-800 dark:text-red-300">Failed to send</h4>
                </div>
                <p class="text-red-700 dark:text-red-400">To proceed, please fill in the necessary fields.</p>
              </div>
            {/if}
            
            <form bind:this={form} on:submit|preventDefault={handleSubmit} class="space-y-6">
              <div>
                <label for="name" class="block text-sm font-medium mb-2">Name</label>
                <input 
                  type="text" 
                  id="name" 
                  name="name"
                  bind:value={name}
                  class="w-full px-4 py-3 border border-gray-300 dark:border-gray-600 rounded-xl focus:ring-2 focus:ring-primary focus:border-primary dark:bg-gray-700 dark:text-white transition-colors duration-200"
                  placeholder="Your name"
                />
              </div>
              
              <div>
                <label for="email" class="block text-sm font-medium mb-2">Email</label>
                <input 
                  type="email" 
                  id="email" 
                  name="email"
                  bind:value={email}
                  class="w-full px-4 py-3 border border-gray-300 dark:border-gray-600 rounded-xl focus:ring-2 focus:ring-primary focus:border-primary dark:bg-gray-700 dark:text-white transition-colors duration-200"
                  placeholder="your.email@example.com"
                />
              </div>
              
              <div>
                <label for="message" class="block text-sm font-medium mb-2">Message</label>
                <textarea 
                  id="message" 
                  name="message"
                  bind:value={message}
                  rows="5"
                  class="w-full px-4 py-3 border border-gray-300 dark:border-gray-600 rounded-xl focus:ring-2 focus:ring-primary focus:border-primary dark:bg-gray-700 dark:text-white transition-colors duration-200"
                  placeholder="Your message here..."
                ></textarea>
              </div>
              
              <button 
                type="submit"
                class="w-full px-6 py-3 bg-primary text-white rounded-xl hover:bg-primary/90 transition-all duration-300 shadow-lg hover:shadow-xl hover:translate-y-[-2px] font-medium active:scale-95"
              >
                Send Message
              </button>
            </form>            
          </div>
        </div>
      </div>
    </section>
  
    <!-- Footer -->
    <footer class="py-12 md:pl-10 bg-gray-900 text-white [@media(prefers-color-scheme:light)]:bg-gray-200 [@media(prefers-color-scheme:light)]:text-black">
      <div class="container mx-auto px-6">
        <div class="flex flex-col md:flex-row justify-between items-center mb-10">
          <div class="mb-8 md:mb-0 text-center md:text-left">
            <a href="#home" class="text-3xl font-mono font-bold inline-block relative group">
              <span class="relative z-10">Want to know more?
              </span>
              <span class="absolute bottom-0 left-0 w-full h-3 bg-primary/30 rounded-lg -z-0 group-hover:h-4 transition-all duration-300"></span>
            </a>
            <p class="text-gray-400 mt-3 max-w-md [@media(prefers-color-scheme:light)]:text-gray-600">Hit me up on my socials to connect and let's collaborate and build something exciting...</p>
          </div>
        </div>
        
        <div class="border-t border-gray-800 pt-8 flex flex-col-reverse md:flex-row justify-between items-center">
          <p class="text-gray-400 text-sm mt-4 md:mt-0 [@media(prefers-color-scheme:light)]:text-gray-500">
            &copy; {new Date().getFullYear()} Om Aryan. All rights reserved.
          </p>
        
          <p class="mb-4 md:mb-0 md:pr-10 flex items-center text-gray-400 text-sm [@media(prefers-color-scheme:light)]:text-gray-500">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-2 mt-1 md:mt-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 5h16a1 1 0 011 1v10a1 1 0 01-1 1H4a1 1 0 01-1-1V6a1 1 0 011-1zm16 12H4m6 0v2m4-2v2" />
            </svg>
            Theme adapts to your system settings.
          </p>
        </div>  
      </div>
    </footer>
  </div>

<style>
  
  :root {
    --color-primary: #00dc82;
    --color-primary-foreground: #f8fafc;
    scroll-behavior: smooth;
  }
  ::selection {
  color: #00ff95;
  background: #001148;
  }

  .highlight {
    background: linear-gradient(to bottom, transparent 20%, #00c87599 90%, #00ff95 60%);
    color: #ffffff;
    position: relative;
    border-radius: 1px;
    animation: flicker 3s linear forwards;
    animation-delay: 1.5s;
    filter: brightness(0.3);
  }
  .highlight::after {
    content: '';
    position: absolute;
    bottom: 0;
  }
  @keyframes flicker {
    0% {
      filter: brightness(0);
      animation-timing-function: step-end;
    }
    2% {
      filter: brightness(0.8);
      animation-timing-function: step-end;
    }
    3% {
      filter: brightness(0);
      animation-timing-function: step-end;
    }
    5% {
      filter: brightness(0.6);
      animation-timing-function: step-end;
    }
    6% {
      filter: brightness(0);
      animation-timing-function: step-end;
    }
    8% {
      filter: brightness(0.9);
      animation-timing-function: step-end;
    }
    10% {
      filter: brightness(0);
      animation-timing-function: step-end;
    }
    15% {
      filter: brightness(1);
      animation-timing-function: step-end;
    }
    20% {
      filter: brightness(0.3);
      animation-timing-function: step-end;
    }
    22% {
      filter: brightness(1);
    }
    100% {
      filter: brightness(1);
    }
  }
  
.gradient-border {
    --borderWidth: 4px;
    position: relative;
    border-radius: 50%;
    background: transparent;
  }

.gradient-border::after {
  content: '';
  position: absolute;
  top: calc(-1 * var(--borderWidth));
  left: calc(-1 * var(--borderWidth));
  width: calc(100% + var(--borderWidth) * 2);
  height: calc(100% + var(--borderWidth) * 2);
  border-radius: 50%;
  background: linear-gradient(
    210deg,
    #00ff95,#004528
  );
  background-size: 300% 300%;
  transform-origin: center center;
  animation: animatedgradient 4s ease infinite;
  z-index: -1;   
}

@keyframes animatedgradient {
  to { transform: rotate(360deg); }
  }

  .dark {
    --color-primary: #818cf8;
    --color-primary-foreground: #f8fafc;
  }
  
  .text-primary {
    color: var(--color-primary);
  }
  
  .text-primary-foreground {
    color: var(--color-primary-foreground);
  }
  
  .bg-primary {
    background-color: var(--color-primary);
  }
  
  .bg-primary\/5 {
    background-color: rgba(99, 102, 241, 0.05);
  }
  
  .bg-primary\/10 {
    background-color: rgba(99, 102, 241, 0.1);
  }
  
  .bg-primary\/20 {
    background-color: rgba(99, 102, 241, 0.2);
  }
  
  .bg-primary\/30 {
    background-color: rgba(99, 102, 241, 0.3);
  }
  
  .bg-primary\/90 {
    background-color: rgba(0, 245, 110, 0.9);
  }
  
  .border-primary {
    border-color: var(--color-primary);
  }
  
  .border-primary\/20 {
    border-color: rgba(99, 102, 241, 0.2);
  }
  
  .border-primary\/30 {
    border-color: rgba(99, 102, 241, 0.3);
  }
  
  .focus\:ring-primary:focus {
    --tw-ring-color: var(--color-primary);
  }
  
  .focus\:border-primary:focus {
    border-color: var(--color-primary);
  }
  
  section {
    scroll-margin-top: 5rem;
  }
  
  .in-view {
    animation: fadeInUp 0.6s ease-out forwards;
  }
  
  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .drawer {
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: 400px;
    margin: 2rem auto;
    position: relative;
    max-height: 400px;
  }

  /* Base tile styling */
  .tile {
    background: #010d21;
    padding: 1rem;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    margin-top: -60px;      /* Overlap tiles for the drawer effect */
    transition: transform 0.3s ease, margin-top 0.3s ease;
    position: relative;
    z-index: 1;
    max-height: 300px;
    overflow: hidden;
  }
  /* First tile should not have a negative margin */
  .tile:first-child {
    margin-top: 0;
  }

  /* On hover, slide the tile upward */
  .tile.hovered {
    transform: translateY(-20px);
    z-index: 2;
    max-height: 320px; /* Use max-height for smoother transitions */
    overflow: visible; /* Ensure content is visible when hovered */
  }

  /* All tiles below the hovered tile adjust their margin so the gap is visible */
  .tile.shifted {
    margin-top: 0;
  }
  /* Reveal details when hovering over a tile */
  .tile.hovered .file-details {
    opacity: 1;
  }

  /* Responsive adjustments */
  @media (max-width: 600px) {
    .drawer {
      max-width: 90%;
      margin: 1rem auto;
    }
    .tile {
      padding: 0.75rem;
      margin-top: -36px;
    }
  }

  @media (prefers-color-scheme: light) {
    .tile {
      background: #ffffff7a;
    }
    .highlight{
      background: linear-gradient(transparent 40%, #eaecff 100%);
      color: #00dc82;
      position: relative;
      border-radius: 5px;
      filter: brightness(1);
      animation: none;
    }
    ::selection {
      color: #00084c;
      background: #00ff95;
    }
    .gradient-border::after {
      content: '';
      position: absolute;
      top: calc(-1 * var(--borderWidth));
      left: calc(-1 * var(--borderWidth));
      width: calc(100% + var(--borderWidth) * 2);
      height: calc(100% + var(--borderWidth) * 2);
      border-radius: 50%;
      background: linear-gradient(
        60deg,
        #00ff95,#ffffff
      );
      background-size: 300% 300%;
      transform-origin: center center;
      animation: animatedgradient 2s ease infinite;
      z-index: -1;
    }
  }
  
  ::-webkit-scrollbar {
    width: 10px;
  }
  
  ::-webkit-scrollbar-track {
    background: #f1f1f1;
  }
  
  .dark ::-webkit-scrollbar-track {
    background: #1f2937;
  }
  
  ::-webkit-scrollbar-thumb {
    background: #c7c7c7;
    border-radius: 5px;
  }
  
  .dark ::-webkit-scrollbar-thumb {
    background: #4b5563;
  }
  
  ::-webkit-scrollbar-thumb:hover {
    background: #a3a3a3;
  }
  
  .dark ::-webkit-scrollbar-thumb:hover {
    background: #6b7280;
  }
</style>