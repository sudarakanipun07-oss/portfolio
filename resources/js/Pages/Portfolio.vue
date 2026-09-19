<template>
    <div class="font-sans text-[#111111] bg-[#EBE5D9] selection:bg-[#FF6B00] selection:text-white overflow-x-hidden relative">
        
        <!-- SIDEBAR OVERLAY -->
        <div class="fixed inset-0 z-[60] pointer-events-none">
            <div class="absolute inset-0 bg-black/5 transition-opacity duration-500" 
                 :class="isSidebarOpen ? 'opacity-100 pointer-events-auto' : 'opacity-0'"
                 @click="isSidebarOpen = false"></div>
            
            <div class="absolute top-0 right-0 w-[80%] max-w-[320px] md:w-80 h-full bg-transparent backdrop-blur-xl border-l border-white/20 shadow-2xl transition-transform duration-500 ease-in-out pointer-events-auto flex flex-col p-8 md:p-10"
                 :class="isSidebarOpen ? 'translate-x-0' : 'translate-x-full'">
                
                <button @click="isSidebarOpen = false" class="self-end mb-10 md:mb-12 hover:text-[#FF6B00] transition-colors duration-300" :class="isDarkBg ? 'text-white' : 'text-[#111111]'">
                    <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
                
                <div class="flex flex-col gap-6 md:gap-8 text-base md:text-lg font-black tracking-widest uppercase transition-colors duration-300" :class="isDarkBg ? 'text-white' : 'text-[#111111]'">
                    <a href="#" @click.prevent="scrollTo('home')" class="hover:text-[#FF6B00] transition-colors relative group w-max">Home<span class="absolute -bottom-2 left-0 w-0 h-0.5 bg-[#FF6B00] transition-all duration-300 group-hover:w-full"></span></a>
                    <a href="#" @click.prevent="scrollTo('about')" class="hover:text-[#FF6B00] transition-colors relative group w-max">About Me<span class="absolute -bottom-2 left-0 w-0 h-0.5 bg-[#FF6B00] transition-all duration-300 group-hover:w-full"></span></a>
                    <a href="#" @click.prevent="scrollTo('projects')" class="hover:text-[#FF6B00] transition-colors relative group w-max">Projects<span class="absolute -bottom-2 left-0 w-0 h-0.5 bg-[#FF6B00] transition-all duration-300 group-hover:w-full"></span></a>
                    <a href="#" @click.prevent="scrollTo('contact')" class="hover:text-[#FF6B00] transition-colors relative group w-max">Contact<span class="absolute -bottom-2 left-0 w-0 h-0.5 bg-[#FF6B00] transition-all duration-300 group-hover:w-full"></span></a>
                </div>
            </div>
        </div>

        <!-- MAIN NAVBAR -->
        <nav class="fixed top-0 left-0 right-0 z-[55] flex items-start justify-between px-4 sm:px-6 md:px-12 max-w-[1600px] mx-auto w-full pointer-events-none">
            
            <div class="hidden xl:block w-[180px]"></div>
            <div class="xl:hidden w-12"></div>
            
            <div class="hidden md:flex gap-10 flex-1 justify-center">
                <div v-for="(link, i) in ['Home', 'About Me', 'Projects', 'Contact']" :key="link" 
                     class="relative flex flex-col items-center origin-top transition-transform duration-700 pointer-events-auto"
                     :class="{ 'animate-swing': isScrolled }"
                     :style="isScrolled ? `animation-delay: ${i * 0.15}s; animation-duration: ${2.5 + (i % 2) * 0.4}s;` : ''">
                    
                    <div class="w-[1.5px] transition-all duration-700 ease-bounce" 
                         :class="[isScrolled ? 'opacity-100' : 'h-0 opacity-0', isAtAboutSection ? 'bg-white/80' : 'bg-[#111111]/40']"
                         :style="isScrolled ? 'height: calc(var(--nav-string-height, 30px) + env(safe-area-inset-top));' : ''"></div>
                         
                    <div class="w-1.5 h-1.5 rounded-full -mt-[1px] z-10 transition-all duration-700 ease-bounce" 
                         :class="[isScrolled ? 'opacity-100 scale-100' : 'opacity-0 scale-0', isAtAboutSection ? 'bg-white' : 'bg-[#111111]']"></div>
                         
                    <a :href="link === 'About Me' ? '#about' : `#${link.toLowerCase()}`" @click.prevent="scrollTo(link === 'About Me' ? 'about' : link.toLowerCase())" 
                       class="text-xs font-bold tracking-widest uppercase transition-all duration-700 whitespace-nowrap" 
                       :class="isScrolled ? (isAtAboutSection ? 'bg-white text-[#111111] px-5 py-2 rounded-full mt-1 shadow-lg hover:bg-[#FF6B00] hover:text-white' : 'bg-[#111111] text-white px-5 py-2 rounded-full mt-1 border border-white/10 hover:bg-[#FF6B00] shadow-lg') : 'text-[#111111] hover:text-[#FF6B00] bg-transparent px-0 py-0 mt-3'">
                        {{ link }}
                    </a>
                </div>
            </div>

            <div class="flex items-start gap-3 md:gap-4 pointer-events-auto mt-2 md:mt-0 transition-transform duration-700 w-auto md:w-[180px] justify-end" :class="{ 'md:mt-0': !isScrolled, 'md:translate-y-3': isScrolled }">
                
                <div class="flex flex-col items-center origin-top transition-transform duration-700" :class="{ 'md:animate-swing': isScrolled }" style="animation-delay: 0.3s; animation-duration: 2.8s;">
                    <div class="hidden md:block w-[1.5px] transition-all duration-700 ease-bounce" 
                         :class="[isScrolled ? 'opacity-100' : 'h-0 opacity-0', isAtAboutSection ? 'bg-white/80' : 'bg-[#111111]/40']"
                         :style="isScrolled ? 'height: calc(var(--nav-string-height, 40px) + env(safe-area-inset-top));' : ''"></div>
                         
                    <div class="hidden md:block w-2 h-2 rounded-full bg-[#FF6B00] -mt-[1px] z-10 transition-all duration-700 ease-bounce" :class="isScrolled ? 'opacity-100 scale-100' : 'opacity-0 scale-0'"></div>
                    
                    <button @click="openModal" class="bg-[#111111] text-white text-[10px] md:text-xs font-black tracking-[0.15em] px-4 md:px-6 py-2.5 md:py-3.5 rounded-full flex items-center gap-2 hover:bg-[#FF6B00] md:hover:-translate-y-0.5 transition-all shadow-lg border border-white/10 mt-1">
                        HIRE NOW
                        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 19L19 5M19 5v10M19 5H9" />
                        </svg>
                    </button>
                </div>

                <div class="flex flex-col items-center origin-top transition-transform duration-700" :class="{ 'md:animate-swing': isScrolled }" style="animation-delay: 0.5s; animation-duration: 3.2s;">
                    <div class="hidden md:block w-[1.5px] transition-all duration-700 ease-bounce" 
                         :class="[isScrolled ? 'opacity-100' : 'h-0 opacity-0', isAtAboutSection ? 'bg-white/80' : 'bg-[#111111]/40']"
                         :style="isScrolled ? 'height: calc(var(--nav-string-height, 40px) + env(safe-area-inset-top));' : ''"></div>
                         
                    <div class="hidden md:block w-2 h-2 rounded-full bg-[#FF6B00] -mt-[1px] z-10 transition-all duration-700 ease-bounce" :class="isScrolled ? 'opacity-100 scale-100' : 'opacity-0 scale-0'"></div>
                    
                    <button @click="isSidebarOpen = true" class="transition-all duration-700 ease-bounce flex items-center justify-center bg-white text-[#111111] p-2.5 md:p-3 rounded-full shadow-lg border border-gray-100 hover:bg-[#FF6B00] hover:text-white mt-1">
                        <svg class="transition-all duration-700 w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                        </svg>
                    </button>
                </div>
            </div>
        </nav>

        <!-- 2. HERO SECTION -->
        <section id="home" class="relative flex flex-col items-center justify-center min-h-screen overflow-hidden">
            <div class="absolute w-full top-[30%] md:top-[35%] -translate-y-1/2 left-0 z-[15] pointer-events-none flex justify-center items-center h-[2px]">
                <div class="absolute right-1/2 w-[45vw] md:w-[42vw] h-[1.5px] bg-white opacity-70 animate-line-left shadow-[0_0_8px_rgba(255,255,255,0.8)]"></div>
                <div class="absolute left-1/2 w-[45vw] md:w-[42vw] h-[1.5px] bg-white opacity-70 animate-line-right shadow-[0_0_8px_rgba(255,255,255,0.8)]"></div>
            </div>

            <div class="absolute inset-0 z-10 pointer-events-none select-none">
                <div class="absolute left-[4%] sm:left-[5%] md:left-[8%] top-[30%] md:top-[35%] -translate-y-1/2 text-left scale-[0.8] sm:scale-100 origin-left">
                    <transition-group name="slow-sort" tag="h2" class="text-xl sm:text-3xl md:text-4xl font-black tracking-tighter text-[#111111] uppercase whitespace-nowrap flex">
                        <span v-for="letter in leftLine1" :key="letter.id" class="inline-block letter-fly" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300" :class="{ 'animate-wiggle-infinite text-[#FF6B00] scale-[1.15] -translate-y-2': letter.isHovered, 'animate-confused text-[#FF6B00]': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <transition-group name="slow-sort" tag="h2" class="text-xl sm:text-3xl md:text-4xl font-black tracking-tighter text-[#111111] uppercase whitespace-nowrap flex mt-[-5px]">
                        <span v-for="letter in leftLine2" :key="letter.id" class="inline-block letter-fly" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300" :class="{ 'animate-wiggle-infinite text-[#FF6B00] scale-[1.15] -translate-y-2': letter.isHovered, 'animate-confused text-[#FF6B00]': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                </div>

                <div class="absolute right-[4%] sm:right-[5%] md:right-[8%] top-[30%] md:top-[35%] -translate-y-1/2 text-right flex flex-col items-end scale-[0.8] sm:scale-100 origin-right">
                    <transition-group name="slow-sort" tag="h2" class="text-2xl sm:text-3xl md:text-5xl font-black tracking-tighter text-[#111111] uppercase whitespace-nowrap flex">
                        <span v-for="letter in rightLine1" :key="letter.id" class="inline-block letter-fly" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300" :class="{ 'animate-wiggle-infinite text-[#FF6B00] scale-[1.15] -translate-y-2': letter.isHovered, 'animate-confused text-[#FF6B00]': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <transition-group name="slow-sort" tag="h2" class="text-2xl sm:text-3xl md:text-5xl font-black tracking-tighter text-[#111111] uppercase whitespace-nowrap flex mt-[-5px]">
                        <span v-for="letter in rightLine2" :key="letter.id" class="inline-block letter-fly" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300" :class="{ 'animate-wiggle-infinite text-[#FF6B00] scale-[1.15] -translate-y-2': letter.isHovered, 'animate-confused text-[#FF6B00]': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <div class="mt-4 h-9"></div>
                </div>

                <div class="absolute w-full top-[68%] md:top-[68%] -translate-y-1/2 text-center flex flex-col items-center justify-center scale-[1.2] md:scale-100">
                    <transition-group name="slow-sort" tag="h1" class="text-[14vw] md:text-[10vw] font-black leading-[0.85] tracking-tighter uppercase whitespace-nowrap flex">
                        <span v-for="letter in line1" :key="letter.id" class="inline-block letter-fly" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300" :class="{ 'animate-wiggle-infinite text-[#FF6B00] scale-[1.15] -translate-y-5': letter.isHovered, 'animate-confused text-[#FF6B00]': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <transition-group name="slow-sort" tag="h1" class="text-[14vw] md:text-[10vw] font-black leading-[0.85] tracking-tighter uppercase whitespace-nowrap flex">
                        <span v-for="letter in line2" :key="letter.id" class="inline-block letter-fly" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300" :class="{ 'animate-wiggle-infinite text-[#FF6B00] scale-[1.15] -translate-y-5': letter.isHovered, 'animate-confused text-[#FF6B00]': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                </div>
            </div>

            <div class="absolute bottom-0 z-20 flex items-end justify-center w-full max-w-[1300px] h-[85vh] md:h-[92vh] pointer-events-none">
                <img src="/images/me.png" alt="Developer Hero" class="object-contain object-bottom w-full h-full drop-shadow-2xl scale-[1.1] md:scale-[1.25]" style="-webkit-mask-image: linear-gradient(to right, transparent 0%, black 15%, black 85%, transparent 100%); mask-image: linear-gradient(to right, transparent 0%, black 15%, black 85%, transparent 100%);" />
            </div>

            <div class="absolute inset-0 z-30 pointer-events-none select-none">
                <div class="absolute left-[4%] sm:left-[5%] md:left-[8%] top-[30%] md:top-[35%] -translate-y-1/2 text-left scale-[0.8] sm:scale-100 origin-left">
                    <transition-group name="slow-sort" tag="h2" class="text-xl sm:text-3xl md:text-4xl font-black tracking-tighter uppercase whitespace-nowrap flex">
                        <span v-for="letter in leftLine1" :key="letter.id" class="inline-block letter-fly pointer-events-none" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300 pointer-events-auto cursor-pointer letter-front" :class="{ 'animate-wiggle-infinite scale-[1.15] -translate-y-2 is-active': letter.isHovered, 'animate-confused is-active': letter.isConfused, 'animate-happy-jump is-active': letter.isHappy }" @mouseenter="letter.isHovered = true" @mouseleave="letter.isHovered = false" @touchstart="letter.isHovered = true" @touchend="letter.isHovered = false">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <transition-group name="slow-sort" tag="h2" class="text-xl sm:text-3xl md:text-4xl font-black tracking-tighter uppercase whitespace-nowrap flex mt-[-5px]">
                        <span v-for="letter in leftLine2" :key="letter.id" class="inline-block letter-fly pointer-events-none" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300 pointer-events-auto cursor-pointer letter-front" :class="{ 'animate-wiggle-infinite scale-[1.15] -translate-y-2 is-active': letter.isHovered, 'animate-confused is-active': letter.isConfused, 'animate-happy-jump is-active': letter.isHappy }" @mouseenter="letter.isHovered = true" @mouseleave="letter.isHovered = false" @touchstart="letter.isHovered = true" @touchend="letter.isHovered = false">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                </div>

                <div class="absolute right-[4%] sm:right-[5%] md:right-[8%] top-[30%] md:top-[35%] -translate-y-1/2 text-right flex flex-col items-end scale-[0.8] sm:scale-100 origin-right">
                    <transition-group name="slow-sort" tag="h2" class="text-2xl sm:text-3xl md:text-5xl font-black tracking-tighter uppercase whitespace-nowrap flex">
                        <span v-for="letter in rightLine1" :key="letter.id" class="inline-block letter-fly pointer-events-none" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300 pointer-events-auto cursor-pointer letter-front" :class="{ 'animate-wiggle-infinite scale-[1.15] -translate-y-2 is-active': letter.isHovered, 'animate-confused is-active': letter.isConfused, 'animate-happy-jump is-active': letter.isHappy }" @mouseenter="letter.isHovered = true" @mouseleave="letter.isHovered = false" @touchstart="letter.isHovered = true" @touchend="letter.isHovered = false">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <transition-group name="slow-sort" tag="h2" class="text-2xl sm:text-3xl md:text-5xl font-black tracking-tighter uppercase whitespace-nowrap flex mt-[-5px]">
                        <span v-for="letter in rightLine2" :key="letter.id" class="inline-block letter-fly pointer-events-none" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300 pointer-events-auto cursor-pointer letter-front" :class="{ 'animate-wiggle-infinite scale-[1.15] -translate-y-2 is-active': letter.isHovered, 'animate-confused is-active': letter.isConfused, 'animate-happy-jump is-active': letter.isHappy }" @mouseenter="letter.isHovered = true" @mouseleave="letter.isHovered = false" @touchstart="letter.isHovered = true" @touchend="letter.isHovered = false">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    
                    <a href="/Nipun_CV.pdf" download="Nipun_Sudaraka_CV.pdf" class="mt-4 px-4 md:px-6 py-2 md:py-2.5 text-[10px] md:text-xs font-black tracking-wider text-white uppercase transition-colors bg-[#111111] rounded hover:bg-[#FF6B00] opacity-0 anim-fade-in-cv pointer-events-auto shadow-[0_4px_15px_rgba(0,0,0,0.1)] hover:-translate-y-0.5 duration-300 flex items-center gap-2">
                        Download CV
                        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
                        </svg>
                    </a>
                </div>

                <div class="absolute w-full top-[68%] md:top-[68%] -translate-y-1/2 text-center flex flex-col items-center justify-center scale-[1.2] md:scale-100">
                    <transition-group name="slow-sort" tag="h1" class="text-[14vw] md:text-[10vw] font-black leading-[0.85] tracking-tighter uppercase whitespace-nowrap flex">
                        <span v-for="letter in line1" :key="letter.id" class="inline-block letter-fly pointer-events-none" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300 pointer-events-auto cursor-pointer letter-front" :class="{ 'animate-wiggle-infinite scale-[1.15] -translate-y-5 is-active': letter.isHovered, 'animate-confused is-active': letter.isConfused, 'animate-happy-jump is-active': letter.isHappy }" @mouseenter="letter.isHovered = true" @mouseleave="letter.isHovered = false" @touchstart="letter.isHovered = true" @touchend="letter.isHovered = false">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                    <transition-group name="slow-sort" tag="h1" class="text-[14vw] md:text-[10vw] font-black leading-[0.85] tracking-tighter uppercase whitespace-nowrap flex">
                        <span v-for="letter in line2" :key="letter.id" class="inline-block letter-fly pointer-events-none" :style="`transform: translate(${letter.x}px, ${letter.y}px);`"><span class="inline-block transition-all duration-300 pointer-events-auto cursor-pointer letter-front" :class="{ 'animate-wiggle-infinite scale-[1.15] -translate-y-5 is-active': letter.isHovered, 'animate-confused is-active': letter.isConfused, 'animate-happy-jump text-[#FF6B00]': letter.isHappy }" @mouseenter="letter.isHovered = true" @mouseleave="letter.isHovered = false" @touchstart="letter.isHovered = true" @touchend="letter.isHovered = false">{{ letter.char === ' ' ? '\u00A0' : letter.char }}</span></span>
                    </transition-group>
                </div>
            </div>

            <a href="#" @click.prevent="scrollTo('about')" class="absolute z-40 flex items-center justify-center w-20 h-20 md:w-24 md:h-24 bottom-4 md:-bottom-12 group pointer-events-auto">
                <div class="absolute inset-0 bg-[#111111] rounded-full transition-transform group-hover:scale-105"></div>
                <div class="absolute inset-2 border border-white/20 rounded-full animate-spin-slow flex items-center justify-center">
                    <svg class="w-full h-full text-white/50" viewBox="0 0 100 100">
                        <path id="curve" d="M 50 10 A 40 40 0 1 1 49.9 10" fill="transparent" />
                        <text class="text-[11px] font-bold tracking-widest uppercase" fill="currentColor"><textPath href="#curve">Scroll Down • Scroll Down • </textPath></text>
                    </svg>
                </div>
                <div class="relative z-10 flex items-center justify-center w-8 h-12 md:w-10 md:h-14 border-2 border-[#FF6B00] rounded-full text-[#FF6B00]">
                    <span class="w-1 h-2 md:h-3 bg-[#FF6B00] rounded-full animate-bounce"></span>
                </div>
            </a>
        </section>

        <!-- 3. ABOUT & EDUCATION SECTION -->
        <section id="about" class="relative px-6 py-20 md:py-32 bg-[#1A1A1A] text-white lg:px-16 z-30 overflow-hidden">
            <div class="max-w-[1400px] mx-auto grid grid-cols-1 lg:grid-cols-2 gap-12 lg:gap-16 items-center">
                
                <div class="flex flex-col relative z-20">
                    <p class="mb-2 text-xs md:text-sm font-bold tracking-widest text-gray-500 uppercase">Hello</p>
                    <h2 class="mb-6 text-4xl md:text-5xl lg:text-7xl font-black tracking-tighter uppercase">I'm <span class="text-[#FF6B00]">Nipun</span></h2>
                    
                    <p class="mb-10 md:mb-14 text-gray-400 font-medium leading-relaxed max-w-lg text-sm md:text-base scroll-animate opacity-0 transform translate-y-8 transition-all duration-700">
                        I'm Nipun Sudaraka, a tech enthusiast focused on software engineering and UI/UX design. I enjoy building clean, scalable digital products and crafting modern user experiences.
                    </p>

                    <h3 class="mb-6 md:mb-8 text-xs md:text-sm font-bold tracking-widest text-gray-500 uppercase scroll-animate opacity-0 transform translate-y-8 transition-all duration-700">Education</h3>
                    
                    <div class="relative pl-6 md:pl-8 border-l-2 border-gray-800 space-y-10 md:space-y-12">
                        <div class="timeline-item relative scroll-animate opacity-0 transform translate-y-8 transition-all duration-700 delay-100">
                            <div class="absolute w-3 h-3 md:w-4 md:h-4 rounded-full transition-all duration-500 -left-[29px] md:-left-[41px] top-1 md:top-1" :class="timelineActive[0] ? 'bg-[#FF6B00] shadow-[0_0_12px_#FF6B00]' : 'bg-gray-600'"></div>
                            <h4 class="text-base md:text-xl font-bold text-white mb-1 transition-colors duration-500" :class="timelineActive[0] ? 'text-white' : 'text-gray-400'">Higher National Diploma in Information Technology (HNDIT)</h4>
                            <p class="text-xs md:text-sm font-semibold mb-2 md:mb-3 transition-colors duration-500" :class="timelineActive[0] ? 'text-[#FF6B00]' : 'text-gray-500'">2024 - Present <span class="text-gray-500 ml-2 font-medium">| SLIATE</span></p>
                            <p class="text-gray-400 text-[11px] md:text-sm leading-relaxed max-w-xl transition-opacity duration-500" :class="timelineActive[0] ? 'opacity-100' : 'opacity-60'">
                                <span class="block mb-1 md:mb-2 font-medium text-white/80">Currently undergraduate, planning to pursue a Top-up degree.</span>
                                • Relevant coursework: Software Development, Object Oriented Programming, Web Programming, Database Management Systems, System Analysis and Design.
                            </p>
                        </div>
                        <div class="timeline-item relative scroll-animate opacity-0 transform translate-y-8 transition-all duration-700 delay-200">
                            <div class="absolute w-3 h-3 md:w-4 md:h-4 rounded-full transition-all duration-500 -left-[29px] md:-left-[41px] top-1" :class="timelineActive[1] ? 'bg-[#FF6B00] shadow-[0_0_12px_#FF6B00]' : 'bg-gray-600'"></div>
                            <h4 class="text-sm md:text-lg font-bold mb-1 transition-colors duration-500" :class="timelineActive[1] ? 'text-white' : 'text-gray-400'">G.C.E. (A/L) Examination</h4>
                            <p class="text-[11px] md:text-sm font-medium transition-colors duration-500" :class="timelineActive[1] ? 'text-gray-300' : 'text-gray-500'">2020 - 2022 (2023) <span class="text-gray-500 ml-2">| Ke/Mw Ashoka Maha Vidyalaya</span></p>
                        </div>
                        <div class="timeline-item relative scroll-animate opacity-0 transform translate-y-8 transition-all duration-700 delay-300">
                            <div class="absolute w-3 h-3 md:w-4 md:h-4 rounded-full transition-all duration-500 -left-[29px] md:-left-[41px] top-1" :class="timelineActive[2] ? 'bg-[#FF6B00] shadow-[0_0_12px_#FF6B00]' : 'bg-gray-600'"></div>
                            <h4 class="text-sm md:text-lg font-bold mb-1 transition-colors duration-500" :class="timelineActive[2] ? 'text-white' : 'text-gray-400'">G.C.E. (O/L) Examination</h4>
                            <p class="text-[11px] md:text-sm font-medium transition-colors duration-500" :class="timelineActive[2] ? 'text-gray-300' : 'text-gray-500'">2018 <span class="text-gray-500 ml-2">| Ke/Mw Ashoka Maha Vidyalaya</span></p>
                        </div>
                    </div>
                </div>

                <div class="relative flex justify-center lg:justify-end z-40 scroll-animate-img opacity-0 transform translate-x-8 transition-all duration-1000 delay-300 mt-16 lg:mt-0 w-full pl-0 lg:pl-12">
                    
                    <div v-if="showDetails" class="fixed inset-0 z-10 cursor-default" @click.stop="showDetails = false"></div>

                    <div class="relative z-20 flex items-center justify-center w-full max-w-[280px] sm:max-w-[320px] md:max-w-[480px]">
                        
                        <div class="absolute right-[5%] sm:right-[15%] lg:right-[85%] top-1/2 -translate-y-1/2 w-[320px] md:w-[350px] h-[450px] pointer-events-none transition-opacity duration-500 mr-2 md:mr-6 z-0 scale-[0.75] sm:scale-100 origin-right"
                             :class="showDetails ? 'opacity-100' : 'opacity-0'">

                             <svg class="absolute inset-0 w-full h-full overflow-visible z-0" :class="showDetails ? 'opacity-100' : 'opacity-0'">
                                 <path d="M 320 225 C 220 225 180 70 100 70" fill="none" stroke="white" stroke-width="1.5" stroke-dasharray="6 6" class="animate-flow-dash" />
                                 <path d="M 320 225 C 220 225 160 140 85 140" fill="none" stroke="white" stroke-width="1.5" stroke-dasharray="6 6" class="animate-flow-dash" />
                                 <path d="M 320 225 C 200 225 150 210 75 210" fill="none" stroke="white" stroke-width="1.5" stroke-dasharray="6 6" class="animate-flow-dash" />
                                 <path d="M 320 225 C 200 225 140 280 60 280" fill="none" stroke="white" stroke-width="1.5" stroke-dasharray="6 6" class="animate-flow-dash" />
                                 <path d="M 320 225 C 220 225 160 350 75 350" fill="none" stroke="white" stroke-width="1.5" stroke-dasharray="6 6" class="animate-flow-dash" />
                                 <path d="M 320 225 C 220 225 180 410 80 410" fill="none" stroke="white" stroke-width="1.5" stroke-dasharray="6 6" class="animate-flow-dash" />
                             </svg>

                             <div class="absolute p-2.5 flex items-center justify-center transition-transform duration-700 ease-bounce w-[150px] h-[48px]"
                                  :class="showDetails ? 'scale-100 translate-x-0' : 'scale-50 translate-x-[40px] opacity-0'"
                                  style="top: 40px; left: -10px;">
                                 <svg class="absolute inset-0 w-full h-full drop-shadow-md" preserveAspectRatio="none" viewBox="0 0 200 120">
                                     <path d="M 50,100 C 20,100 0,80 0,55 C 0,35 15,15 35,15 C 45,0 70,-5 90,10 C 110,-5 150,0 160,25 C 185,20 200,35 200,60 C 200,85 180,100 150,100 Z" fill="rgba(26,26,26,0.85)" stroke="rgba(255,255,255,0.9)" stroke-width="3" style="backdrop-filter: blur(6px);" />
                                 </svg>
                                 <div class="relative z-10 flex items-center gap-1.5 px-2 py-0.5">
                                     <svg class="w-3.5 h-3.5 text-white shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                         <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
                                     </svg>
                                     <span class="text-[11px] font-bold text-white whitespace-nowrap">Problem Solving</span>
                                 </div>
                             </div>

                             <div class="absolute p-2.5 flex items-center justify-center transition-transform duration-700 ease-bounce delay-75 w-[150px] h-[48px]"
                                  :class="showDetails ? 'scale-100 translate-x-0' : 'scale-50 translate-x-[40px] opacity-0'"
                                  style="top: 110px; left: -25px;">
                                 <svg class="absolute inset-0 w-full h-full drop-shadow-md" preserveAspectRatio="none" viewBox="0 0 200 120">
                                     <path d="M 50,100 C 20,100 0,80 0,55 C 0,35 15,15 35,15 C 45,0 70,-5 90,10 C 110,-5 150,0 160,25 C 185,20 200,35 200,60 C 200,85 180,100 150,100 Z" fill="rgba(26,26,26,0.85)" stroke="rgba(255,255,255,0.9)" stroke-width="3" style="backdrop-filter: blur(6px);" />
                                 </svg>
                                 <div class="relative z-10 flex items-center gap-1.5 px-2 py-0.5">
                                     <svg class="w-3.5 h-3.5 text-white shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                         <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 10h.01M12 10h.01M16 10h.01M9 16H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-5l-5 5v-5z" />
                                     </svg>
                                     <span class="text-[11px] font-bold text-white whitespace-nowrap">Communication</span>
                                 </div>
                             </div>

                             <div class="absolute p-2.5 flex items-center justify-center transition-transform duration-700 ease-bounce delay-150 w-[130px] h-[48px]"
                                  :class="showDetails ? 'scale-100 translate-x-0' : 'scale-50 translate-x-[40px] opacity-0'"
                                  style="top: 180px; left: -30px;">
                                 <svg class="absolute inset-0 w-full h-full drop-shadow-md" preserveAspectRatio="none" viewBox="0 0 200 120">
                                     <path d="M 50,100 C 20,100 0,80 0,55 C 0,35 15,15 35,15 C 45,0 70,-5 90,10 C 110,-5 150,0 160,25 C 185,20 200,35 200,60 C 200,85 180,100 150,100 Z" fill="rgba(26,26,26,0.85)" stroke="rgba(255,255,255,0.9)" stroke-width="3" style="backdrop-filter: blur(6px);" />
                                 </svg>
                                 <div class="relative z-10 flex items-center gap-1.5 px-2 py-0.5">
                                     <svg class="w-3.5 h-3.5 text-white shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                         <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
                                     </svg>
                                     <span class="text-[11px] font-bold text-white whitespace-nowrap">Teamwork</span>
                                 </div>
                             </div>

                             <div class="absolute p-2.5 flex items-center justify-center transition-transform duration-700 ease-bounce delay-[225ms] w-[180px] h-[52px]"
                                  :class="showDetails ? 'scale-100 translate-x-0' : 'scale-50 translate-x-[40px] opacity-0'"
                                  style="top: 250px; left: -45px;">
                                 <svg class="absolute inset-0 w-full h-full drop-shadow-md" preserveAspectRatio="none" viewBox="0 0 200 120">
                                     <path d="M 50,100 C 20,100 0,80 0,55 C 0,35 15,15 35,15 C 45,0 70,-5 90,10 C 110,-5 150,0 160,25 C 185,20 200,35 200,60 C 200,85 180,100 150,100 Z" fill="rgba(26,26,26,0.85)" stroke="rgba(255,255,255,0.9)" stroke-width="3" style="backdrop-filter: blur(6px);" />
                                 </svg>
                                 <div class="relative z-10 flex items-center gap-1.5 px-2 py-0.5">
                                     <svg class="w-3.5 h-3.5 text-white shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                         <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z" />
                                     </svg>
                                     <span class="text-[11px] font-bold text-white whitespace-nowrap">AI Prompting & Handling</span>
                                 </div>
                             </div>

                             <div class="absolute p-2.5 flex items-center justify-center transition-transform duration-700 ease-bounce delay-[300ms] w-[190px] h-[52px]"
                                  :class="showDetails ? 'scale-100 translate-x-0' : 'scale-50 translate-x-[40px] opacity-0'"
                                  style="top: 320px; left: -30px;">
                                 <svg class="absolute inset-0 w-full h-full drop-shadow-md" preserveAspectRatio="none" viewBox="0 0 200 120">
                                     <path d="M 50,100 C 20,100 0,80 0,55 C 0,35 15,15 35,15 C 45,0 70,-5 90,10 C 110,-5 150,0 160,25 C 185,20 200,35 200,60 C 200,85 180,100 150,100 Z" fill="rgba(26,26,26,0.85)" stroke="rgba(255,255,255,0.9)" stroke-width="3" style="backdrop-filter: blur(6px);" />
                                 </svg>
                                 <div class="relative z-10 flex items-center gap-1.5 px-2 py-0.5">
                                     <svg class="w-3.5 h-3.5 text-white shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                         <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" />
                                     </svg>
                                     <span class="text-[11px] font-bold text-white whitespace-nowrap">Deep Passion for Tech</span>
                                 </div>
                             </div>

                             <div class="absolute p-3.5 flex items-center justify-center transition-transform duration-700 ease-bounce delay-[375ms] w-[260px] h-[80px]"
                                  :class="showDetails ? 'scale-100 translate-x-0' : 'scale-50 translate-x-[40px] opacity-0'"
                                  style="top: 385px; left: -50px;">
                                 <svg class="absolute inset-0 w-full h-full drop-shadow-md" preserveAspectRatio="none" viewBox="0 0 200 120">
                                     <path d="M 50,100 C 20,100 0,80 0,55 C 0,35 15,15 35,15 C 45,0 70,-5 90,10 C 110,-5 150,0 160,25 C 185,20 200,35 200,60 C 200,85 180,100 150,100 Z" fill="rgba(26,26,26,0.85)" stroke="rgba(255,255,255,0.9)" stroke-width="3" style="backdrop-filter: blur(6px);" />
                                 </svg>
                                 <div class="relative z-10 flex flex-col items-center gap-1 px-3 text-center">
                                     <svg class="w-4 h-4 text-white shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                         <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 11h2M12 10v2M15 11h2M16 10v2M8 11h.01M5 11h.01M6.5 9.5v3M4 6h16a2 2 0 012 2v8a2 2 0 01-2 2H4a2 2 0 01-2-2V8a2 2 0 012-2z" />
                                     </svg>
                                     <span class="text-[10px] font-medium text-white leading-tight">Playing PC games sparked my tech interest.</span>
                                 </div>
                             </div>
                        </div>

                        <div class="absolute right-[85%] sm:right-[95%] md:right-[105%] top-[40%] -translate-y-1/2 flex items-center transition-opacity duration-300 pointer-events-none mr-2 md:mr-4 scale-[0.8] sm:scale-100"
                             :class="showDetails ? 'opacity-0' : 'opacity-100 animate-wave-float'">
                             <span class="text-[#111111] font-black text-[10px] uppercase tracking-widest mr-2 drop-shadow-md border border-white/40 px-3 py-1.5 rounded-full bg-white/90 backdrop-blur-sm whitespace-nowrap">Click Here</span>
                             <svg width="40" height="20" viewBox="0 0 50 25" class="overflow-visible text-white md:w-[50px] md:h-[25px]">
                                 <path d="M 0 12 Q 25 -8 45 12" fill="none" stroke="currentColor" stroke-width="2" stroke-dasharray="5 5" class="animate-arrow-flow" />
                                 <polygon points="40,7 50,12 40,17" fill="currentColor" />
                             </svg>
                        </div>

                        <div class="w-[260px] sm:w-[320px] md:w-full md:max-w-[400px] relative cursor-pointer group z-30 transition-transform duration-500"
                             :class="showDetails ? 'scale-[1.02] sm:scale-105' : 'hover:scale-[1.02]'"
                             @click.stop="showDetails = !showDetails">
                            
                            <img src="/images/aboutme.jpg" alt="About Me Details"
                                 class="w-full h-auto object-contain rounded-2xl transition-all duration-1000 shadow-2xl"
                                 style="-webkit-mask-image: linear-gradient(to bottom, black 80%, transparent 100%); mask-image: linear-gradient(to bottom, black 80%, transparent 100%);"
                                 :class="showDetails ? 'grayscale-0 mix-blend-normal' : 'grayscale opacity-75 mix-blend-luminosity group-hover:opacity-100'" />
                                 
                            <div class="absolute top-0 right-2 md:right-4 h-full py-4 sm:py-6 md:py-8 z-40 pointer-events-none flex items-center justify-center overflow-hidden transition-opacity duration-500">
                                <span class="vertical-quote text-[9px] sm:text-[10px] md:text-[13px] font-black tracking-[0.25em] whitespace-nowrap scroll-animate opacity-0 transform translate-y-12 transition-all duration-[1200ms] ease-out delay-700">
                                    Engineering digital experiences with intention & artistic precision
                                </span>
                            </div>
                            
                        </div>

                    </div>
                </div>

            </div>
        </section>

        <!-- 3.5 TECHNOLOGIES SCROLLING SECTION -->
        <section class="py-16 md:py-24 bg-white overflow-hidden border-y border-gray-100">
            <div class="max-w-[1400px] mx-auto px-4 md:px-6 mb-12 md:mb-16 text-center">
                <h2 class="text-xl sm:text-2xl md:text-3xl lg:text-4xl font-medium text-gray-500 tracking-tight">Technologies & tools I <span class="font-black text-[#111111]">build & collaborate</span> with.</h2>
            </div>

            <div class="relative w-full flex flex-col border-t border-gray-100 overflow-hidden">
                
                <div class="flex animate-scroll-left w-max hover:[animation-play-state:paused] cursor-pointer border-b border-gray-100">
                    <div v-for="(item, index) in [...row1, ...row1]" :key="'r1'+index" class="flex items-center justify-center gap-3 md:gap-4 w-[160px] md:w-[250px] border-r border-gray-100 py-4 md:py-8 shrink-0 transition-colors hover:bg-gray-50">
                        <div class="w-6 h-6 md:w-8 md:h-8 flex items-center justify-center" v-html="item.svg"></div>
                        <span class="font-bold text-[#111111] text-xs md:text-base">{{ item.name }}</span>
                    </div>
                </div>

                <div class="flex animate-scroll-right w-max hover:[animation-play-state:paused] cursor-pointer border-b border-gray-100">
                    <div v-for="(item, index) in [...row2, ...row2]" :key="'r2'+index" class="flex items-center justify-center gap-3 md:gap-4 w-[160px] md:w-[250px] border-r border-gray-100 py-4 md:py-8 shrink-0 transition-colors hover:bg-gray-50">
                        <div class="w-6 h-6 md:w-8 md:h-8 flex items-center justify-center" v-html="item.svg"></div>
                        <span class="font-bold text-[#111111] text-xs md:text-base">{{ item.name }}</span>
                    </div>
                </div>

                <div class="flex animate-scroll-left w-max hover:[animation-play-state:paused] cursor-pointer border-b border-gray-100" style="animation-duration: 35s;">
                    <div v-for="(item, index) in [...row3, ...row3]" :key="'r3'+index" class="flex items-center justify-center gap-3 md:gap-4 w-[160px] md:w-[250px] border-r border-gray-100 py-4 md:py-8 shrink-0 transition-colors hover:bg-gray-50">
                        <div class="w-6 h-6 md:w-8 md:h-8 flex items-center justify-center" v-html="item.svg"></div>
                        <span class="font-bold text-[#111111] text-xs md:text-base">{{ item.name }}</span>
                    </div>
                </div>

            </div>
        </section>

        <!-- 4. OUR WORKS SECTION -->
        <section id="projects" class="px-4 sm:px-6 py-20 md:py-32 bg-white lg:px-16 overflow-hidden border-t border-gray-100">
            <div class="max-w-[1400px] mx-auto relative">
                
                <div class="flex flex-col xl:flex-row xl:items-end justify-between gap-6 md:gap-8 mb-10 md:mb-12">
                    <div>
                        <h2 class="text-3xl md:text-5xl lg:text-[56px] font-black text-[#111111] tracking-tighter mb-2 flex items-baseline gap-3">
                            Our Works <span class="text-lg md:text-2xl text-gray-400 font-medium tracking-normal">({{ filteredProjects.length }})</span>
                        </h2>
                        <p class="text-gray-500 font-medium text-sm md:text-base">Explore digital journeys designed and developed with precision & creativity.</p>
                    </div>
                    
                    <div class="flex flex-col sm:flex-row items-center gap-4 md:gap-6 w-full xl:w-auto">
                        <div class="flex items-center p-1.5 bg-gray-50 rounded-full border border-gray-100 overflow-x-auto w-full sm:w-auto scrollbar-hide">
                            <button v-for="cat in categories" :key="cat"
                                    @click="activeCategory = cat"
                                    class="px-4 sm:px-6 py-2 sm:py-2.5 text-[11px] sm:text-xs font-bold rounded-full transition-colors whitespace-nowrap"
                                    :class="activeCategory === cat ? 'bg-white text-[#111111] shadow-sm' : 'text-gray-500 hover:text-[#111111]'">
                                {{ cat }}
                            </button>
                        </div>
                        <div class="hidden sm:flex gap-2 shrink-0">
                            <button @click="scrollProjects('left')" class="w-10 h-10 rounded-full border border-gray-200 flex items-center justify-center text-gray-400 hover:text-[#111111] hover:border-gray-300 transition-colors cursor-pointer">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
                                </svg>
                            </button>
                            <button @click="scrollProjects('right')" class="w-10 h-10 rounded-full border border-gray-200 flex items-center justify-center text-gray-400 hover:text-[#111111] hover:border-gray-300 transition-colors cursor-pointer">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>

                <transition-group name="project-list" tag="div" ref="projectsScrollRef" class="flex gap-4 md:gap-6 overflow-x-auto pb-8 md:pb-12 snap-x snap-mandatory scrollbar-hide scroll-smooth" style="scrollbar-width: none; -ms-overflow-style: none;">
                    
                    <div v-for="project in filteredProjects" :key="project.id" class="relative w-[280px] sm:w-[320px] md:w-[380px] h-[450px] md:h-[520px] shrink-0 rounded-[24px] md:rounded-[32px] overflow-hidden group snap-start bg-[#111]">
                        <img :src="project.image" class="absolute inset-0 w-full h-1/2 object-cover opacity-90 group-hover:scale-105 transition-transform duration-700" :alt="project.title">
                        <div class="absolute inset-0 bg-gradient-to-t from-[#0a0a0a] via-[#0a0a0a]/90 to-transparent"></div>
                        
                        <div class="absolute top-4 left-4 md:top-5 md:left-5 px-3 py-1.5 bg-black/40 backdrop-blur-md rounded-full border border-white/10 flex items-center gap-2">
                            <div class="w-1.5 h-1.5 rounded-full bg-[#FF6B00]"></div>
                            <span class="text-[9px] md:text-[10px] font-black text-white uppercase tracking-wider">{{ project.category }}</span>
                        </div>

                        <div class="absolute bottom-0 left-0 w-full p-5 md:p-6 flex flex-col">
                            <h3 class="text-xl md:text-2xl font-bold text-white mb-2 leading-tight">{{ project.title }}</h3>
                            <p class="text-gray-400 text-[11px] md:text-xs leading-relaxed mb-4 md:mb-5 line-clamp-2">{{ project.description }}</p>
                            
                            <div class="flex items-center justify-between mb-5 md:mb-6">
                                <div class="flex items-center gap-1.5 md:gap-2">
                                    <span class="text-white font-bold text-[11px] md:text-xs">{{ project.rating }}</span>
                                    <div class="flex text-yellow-500 text-[9px] md:text-[10px]">★★★★★</div>
                                </div>
                                <div class="flex items-center gap-2">
                                    <div class="flex -space-x-1">
                                        <div v-for="(icon, idx) in project.icons" :key="idx" class="w-5 h-5 md:w-6 md:h-6 rounded-full bg-white/10 flex items-center justify-center p-1 md:p-1.5" v-html="icon"></div>
                                    </div>
                                    <span class="px-2 md:px-2.5 py-1 bg-white/5 border border-white/10 rounded-full text-[9px] md:text-[10px] font-semibold text-gray-300">{{ project.tools }}</span>
                                </div>
                            </div>
                            
                            <a :href="project.link" target="_blank" rel="noopener noreferrer" class="w-full py-3 md:py-3.5 bg-white text-[#111111] rounded-full text-[11px] md:text-xs font-bold hover:bg-[#FF6B00] hover:text-white transition-colors flex justify-center items-center">Explore Now</a>
                        </div>
                    </div>

                    <div v-if="filteredProjects.length === 0" key="empty-state" class="w-full py-12 flex items-center justify-center">
                        <span class="text-gray-400 font-medium text-sm">No projects found in this category.</span>
                    </div>

                </transition-group>
            </div>
        </section>

        <!-- 5. CONTACT & FOOTER SECTION -->
        <section id="contact" class="bg-[#0a0a0a] text-white pt-20 md:pt-24 pb-8 px-4 sm:px-6 lg:px-16 border-t border-white/10">
            <div class="max-w-[1400px] mx-auto">
                
                <!-- Top: Contact Info & Form -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 md:gap-16 lg:gap-24 mb-16 md:mb-24">
                    <!-- Left: Info -->
                    <div class="flex flex-col text-center lg:text-left items-center lg:items-start">
                        <span class="text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-[0.2em] mb-4 md:mb-6">Get in touch</span>
                        <h2 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-bold tracking-tight mb-4 md:mb-6 leading-[1.1]">Let's build<br>something great.</h2>
                        <p class="text-gray-400 text-xs sm:text-sm md:text-base leading-relaxed mb-8 md:mb-10 max-w-sm">
                            Open to freelance projects, collaborations, and full-time opportunities.
                        </p>
                        <div class="flex items-center gap-4 md:gap-6 text-xs md:text-sm font-medium text-gray-400">
                            <a href="https://github.com/nipunsudaraka" target="_blank" class="hover:text-white transition-colors">GitHub</a>
                            <span class="w-[1px] h-4 bg-gray-700"></span>
                            <a href="#" class="hover:text-white transition-colors">LinkedIn</a>
                        </div>
                    </div>

                    <!-- Right: Form -->
                    <div class="flex flex-col gap-4 md:gap-6 w-full max-w-md mx-auto lg:max-w-none">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 md:gap-6">
                            <div class="flex flex-col gap-1.5 md:gap-2">
                                <label class="text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-widest">Name</label>
                                <input v-model="form.name" type="text" placeholder="Your name" class="bg-[#111111] border border-white/10 rounded-xl px-4 py-3 md:py-3.5 text-xs md:text-sm text-white placeholder-gray-600 focus:outline-none focus:border-[#FF6B00] transition-colors w-full">
                            </div>
                            <div class="flex flex-col gap-1.5 md:gap-2">
                                <label class="text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-widest">Email</label>
                                <input v-model="form.email" type="email" placeholder="you@email.com" class="bg-[#111111] border border-white/10 rounded-xl px-4 py-3 md:py-3.5 text-xs md:text-sm text-white placeholder-gray-600 focus:outline-none focus:border-[#FF6B00] transition-colors w-full">
                            </div>
                        </div>
                        <div class="flex flex-col gap-1.5 md:gap-2">
                            <label class="text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-widest">Message</label>
                            <textarea v-model="form.details" placeholder="Tell me about your project..." rows="4" class="bg-[#111111] border border-white/10 rounded-xl px-4 py-3 md:py-3.5 text-xs md:text-sm text-white placeholder-gray-600 focus:outline-none focus:border-[#FF6B00] transition-colors resize-none w-full"></textarea>
                        </div>
                        <button @click="submitFooterForm" class="bg-white text-[#111111] font-bold text-xs md:text-sm px-6 md:px-8 py-3 md:py-3.5 rounded-xl w-full sm:w-max justify-center hover:bg-[#FF6B00] hover:text-white transition-colors flex items-center gap-2 mt-2">
                            Send Message
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                            </svg>
                        </button>
                    </div>
                </div>

                <!-- Bottom: Footer Links & Copyright -->
                <div class="flex flex-col gap-6 md:gap-8">
                    <!-- Middle: Brand & Links -->
                    <div class="flex flex-col md:flex-row items-center justify-between gap-6 pb-6 md:pb-8 border-b border-white/10">
                        <div class="flex items-center gap-3">
                            <img src="/images/me.png" alt="Nipun" class="w-8 h-8 rounded-full object-cover bg-[#FF6B00]">
                            <span class="font-bold text-white tracking-tight text-sm">nipun.sudaraka</span>
                        </div>
                        <div class="flex flex-wrap justify-center gap-4 sm:gap-6 md:gap-8 text-[11px] md:text-xs font-medium text-gray-400">
                            <a href="#" @click.prevent="scrollTo('home')" class="hover:text-white transition-colors">Overview</a>
                            <a href="#" @click.prevent="scrollTo('projects')" class="hover:text-white transition-colors">Work</a>
                            <a href="#" @click.prevent="scrollTo('about')" class="hover:text-white transition-colors">About</a>
                            <a href="https://github.com/nipunsudaraka" target="_blank" class="hover:text-white transition-colors">GitHub</a>
                        </div>
                    </div>

                    <!-- Very Bottom: Copyright & Status -->
                    <div class="flex flex-col md:flex-row items-center justify-between gap-3 text-[10px] md:text-[11px] font-medium text-gray-500 text-center md:text-left">
                        <p>© {{ new Date().getFullYear() }} nipun.sudaraka. All rights reserved.</p>
                        <div class="flex items-center justify-center gap-2">
                            <div class="w-1.5 h-1.5 rounded-full bg-green-500"></div>
                            <span>Sri Lanka · Remote</span>
                        </div>
                        <p>Built by Nipun Sudaraka</p>
                    </div>
                </div>
                
            </div>
        </section>

        <!-- MULTI-STEP HIRE ME MODAL -->
        <transition name="modal-fade">
            <div v-if="isModalOpen" class="fixed inset-0 z-[100] bg-black/60 backdrop-blur-sm flex items-center justify-center p-4">
                <div class="bg-white rounded-[24px] md:rounded-[32px] w-full max-w-2xl shadow-2xl overflow-hidden" @click.stop>
                    
                    <div class="p-5 md:p-8 pb-3 md:pb-4 relative">
                        <button @click="closeModal" class="absolute top-4 right-4 md:top-6 md:right-6 w-8 h-8 bg-gray-100 hover:bg-gray-200 text-gray-600 rounded-full flex items-center justify-center transition-colors">
                            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>
                        
                        <p class="text-[9px] md:text-[10px] font-bold text-[#FF6B00] uppercase tracking-widest mb-1.5 md:mb-2">Step {{ currentStep }} of 3 - Start A Project</p>
                        <h3 class="text-xl md:text-3xl font-black text-[#111111] tracking-tight">
                            {{ currentStep === 1 ? 'Select Required Service' : currentStep === 2 ? 'Estimated Timeline' : 'Choose Inquiry Method' }}
                        </h3>
                    </div>

                    <div class="px-5 md:px-8 pb-5 md:pb-6">
                        <div class="flex items-center justify-between relative mt-2 md:mt-4">
                            <div class="absolute left-0 top-1/2 -translate-y-1/2 w-full h-[2px] bg-gray-100 z-0"></div>
                            <div class="absolute left-0 top-1/2 -translate-y-1/2 h-[2px] bg-[#FF6B00] z-0 transition-all duration-300" :style="{ width: ((currentStep - 1) * 50) + '%' }"></div>
                            
                            <div v-for="step in 3" :key="'step'+step" class="relative z-10 flex flex-col items-center bg-white px-2">
                                <div class="w-6 h-6 md:w-8 md:h-8 rounded-full flex items-center justify-center text-[10px] md:text-xs font-bold transition-colors duration-300"
                                     :class="currentStep > step ? 'bg-[#FF6B00] text-white' : currentStep === step ? 'bg-[#FF6B00] text-white shadow-md shadow-[#FF6B00]/30' : 'bg-gray-100 text-gray-400'">
                                    <span v-if="currentStep <= step">{{ step }}</span>
                                    <svg v-else class="w-3 h-3 md:w-4 md:h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7" />
                                    </svg>
                                </div>
                                <span class="text-[8px] md:text-[9px] font-bold uppercase tracking-widest mt-1.5 md:mt-2 transition-colors duration-300" :class="currentStep >= step ? 'text-[#FF6B00]' : 'text-gray-400'">
                                    {{ step === 1 ? 'Service' : step === 2 ? 'Timeline' : 'Contact' }}
                                </span>
                            </div>
                        </div>
                    </div>

                    <div class="px-5 md:px-8 pb-6 md:pb-8">
                        
                        <div v-if="currentStep === 1" class="grid grid-cols-1 sm:grid-cols-2 gap-2.5 md:gap-3">
                            <button v-for="srv in servicesList" :key="srv.name" @click="form.service = srv.name" 
                                    class="p-3 md:p-4 border rounded-xl md:rounded-2xl flex items-center gap-3 transition-all text-left"
                                    :class="form.service === srv.name ? 'border-[#FF6B00] bg-[#FF6B00]/5 ring-1 ring-[#FF6B00]' : 'border-gray-200 hover:border-[#FF6B00]/50'">
                                <div class="w-7 h-7 md:w-8 md:h-8 rounded-full flex items-center justify-center shrink-0" :class="form.service === srv.name ? 'bg-[#FF6B00]/20 text-[#FF6B00]' : 'bg-gray-50 text-gray-500'">
                                    <div v-html="srv.icon"></div>
                                </div>
                                <span class="font-bold text-xs md:text-sm text-[#111111] flex-1">{{ srv.name }}</span>
                                <div class="w-3.5 h-3.5 md:w-4 md:h-4 rounded-full border-2 flex items-center justify-center shrink-0" :class="form.service === srv.name ? 'border-[#FF6B00]' : 'border-gray-300'">
                                    <div v-if="form.service === srv.name" class="w-1.5 h-1.5 md:w-2 md:h-2 bg-[#FF6B00] rounded-full"></div>
                                </div>
                            </button>
                        </div>

                        <div v-if="currentStep === 2" class="grid grid-cols-1 sm:grid-cols-3 gap-2.5 md:gap-3">
                            <button v-for="time in timelineList" :key="time" @click="form.timeline = time" 
                                    class="p-4 md:p-5 border rounded-xl md:rounded-2xl flex flex-row sm:flex-col items-center justify-between sm:justify-center gap-2 transition-all text-center"
                                    :class="form.timeline === time ? 'border-[#FF6B00] bg-[#FF6B00]/5 ring-1 ring-[#FF6B00]' : 'border-gray-200 hover:border-[#FF6B00]/50'">
                                <span class="font-black text-sm md:text-lg text-[#111111]">{{ time }}</span>
                                <div class="w-3.5 h-3.5 md:w-4 md:h-4 rounded-full border-2 sm:mt-2 flex items-center justify-center" :class="form.timeline === time ? 'border-[#FF6B00]' : 'border-gray-300'">
                                    <div v-if="form.timeline === time" class="w-1.5 h-1.5 md:w-2 md:h-2 bg-[#FF6B00] rounded-full"></div>
                                </div>
                            </button>
                        </div>

                        <div v-if="currentStep === 3" class="animate-fade-in">
                            <div class="mb-4 md:mb-5 flex items-center gap-1.5 p-1 bg-gray-50 rounded-xl border border-gray-100">
                                <button @click="form.contactMethod = 'whatsapp'" class="flex-1 py-2.5 md:py-3 text-[11px] md:text-xs font-bold rounded-lg transition-all flex items-center justify-center gap-1.5 md:gap-2" :class="form.contactMethod === 'whatsapp' ? 'bg-white shadow-sm text-[#FF6B00] border border-gray-200' : 'text-gray-500 hover:text-[#111111]'">
                                    <svg class="w-3.5 h-3.5 md:w-4 md:h-4" fill="currentColor" viewBox="0 0 24 24">
                                        <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51a12.8 12.8 0 00-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z" />
                                    </svg>
                                    WhatsApp Inquiry
                                </button>
                                <button @click="form.contactMethod = 'email'" class="flex-1 py-2.5 md:py-3 text-[11px] md:text-xs font-bold rounded-lg transition-all flex items-center justify-center gap-1.5 md:gap-2" :class="form.contactMethod === 'email' ? 'bg-white shadow-sm text-[#FF6B00] border border-gray-200' : 'text-gray-500 hover:text-[#111111]'">
                                    <svg class="w-3.5 h-3.5 md:w-4 md:h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                                    </svg>
                                    Email Inquiry
                                </button>
                            </div>

                            <div class="mb-4 md:mb-5 p-2.5 md:p-3 rounded-lg border flex items-center gap-2" :class="form.contactMethod === 'whatsapp' ? 'bg-[#25D366]/10 border-[#25D366]/30 text-[#25D366]' : 'bg-[#FF6B00]/10 border-[#FF6B00]/30 text-[#FF6B00]'">
                                <svg v-if="form.contactMethod === 'whatsapp'" class="w-3.5 h-3.5 md:w-4 md:h-4 shrink-0" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51a12.8 12.8 0 00-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z" />
                                </svg>
                                <svg v-else class="w-3.5 h-3.5 md:w-4 md:h-4 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                                </svg>
                                <span class="text-[10px] md:text-xs font-bold">{{ form.contactMethod === 'whatsapp' ? 'Direct to WhatsApp: +94710474475' : 'Direct to Email: sudarakanipun07@gmail.com' }}</span>
                            </div>

                            <div class="mb-3 md:mb-4">
                                <label class="block text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-1 md:mb-1.5">Your Name</label>
                                <input v-model="form.name" type="text" placeholder="e.g. Alex" class="w-full px-3 md:px-4 py-2 md:py-2.5 rounded-lg md:rounded-xl border border-gray-200 focus:border-[#FF6B00] focus:ring-1 focus:ring-[#FF6B00] outline-none transition-all text-xs md:text-sm font-medium" />
                            </div>
                            
                            <div class="mb-3 md:mb-4">
                                <label class="block text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-1 md:mb-1.5">Your Email</label>
                                <input v-model="form.email" type="email" placeholder="e.g. alex@example.com" class="w-full px-3 md:px-4 py-2 md:py-2.5 rounded-lg md:rounded-xl border border-gray-200 focus:border-[#FF6B00] focus:ring-1 focus:ring-[#FF6B00] outline-none transition-all text-xs md:text-sm font-medium" />
                            </div>

                            <div class="mb-3 md:mb-4">
                                <label class="block text-[9px] md:text-[10px] font-bold text-gray-500 uppercase tracking-widest mb-1 md:mb-1.5">Project Details (Optional)</label>
                                <textarea v-model="form.details" placeholder="Briefly describe your project requirements..." rows="3" class="w-full px-3 md:px-4 py-2 md:py-2.5 rounded-lg md:rounded-xl border border-gray-200 focus:border-[#FF6B00] focus:ring-1 focus:ring-[#FF6B00] outline-none transition-all text-xs md:text-sm font-medium resize-none"></textarea>
                            </div>
                        </div>

                        <!-- Footer Actions -->
                        <div class="flex items-center justify-between pt-3 md:pt-4 border-t border-gray-100">
                            <button v-if="currentStep > 1" @click="prevStep" class="px-3 sm:px-5 py-2 md:py-2.5 text-[10px] md:text-xs font-bold text-gray-500 hover:text-[#111111] transition-colors flex items-center gap-1.5">
                                <svg class="w-3 h-3 md:w-3.5 md:h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15 19l-7-7 7-7" />
                                </svg>
                                Back
                            </button>
                            <div v-else></div>

                            <button v-if="currentStep < 3" @click="nextStep" class="px-4 sm:px-6 py-2 md:py-2.5 bg-[#111111] text-white text-[10px] md:text-xs font-bold rounded-full hover:bg-[#FF6B00] transition-colors flex items-center gap-1.5 md:gap-2 shadow-md hover:-translate-y-0.5 w-full sm:w-auto justify-center">
                                Continue
                                <svg class="w-3 h-3 md:w-3.5 md:h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M9 5l7 7-7 7" />
                                </svg>
                            </button>
                            
                            <button v-else @click="submitForm" class="px-4 sm:px-6 py-2 md:py-2.5 text-white text-[10px] md:text-xs font-bold rounded-full transition-colors flex items-center justify-center w-full sm:w-auto gap-1.5 md:gap-2 shadow-md hover:-translate-y-0.5" :class="form.contactMethod === 'whatsapp' ? 'bg-[#25D366] hover:bg-[#20b958]' : 'bg-[#FF6B00] hover:bg-[#e66000]'">
                                <svg v-if="form.contactMethod === 'whatsapp'" class="w-3.5 h-3.5 md:w-4 md:h-4" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51a12.8 12.8 0 00-.57-.01c-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z" />
                                </svg>
                                <svg v-else class="w-3.5 h-3.5 md:w-4 md:h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                                </svg>
                                {{ form.contactMethod === 'whatsapp' ? 'Send via WhatsApp' : 'Send via Email' }}
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </transition>

    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';

const isSidebarOpen = ref(false);
const isScrolled = ref(false);
const showDetails = ref(false);
const scrollY = ref(0);
const projectsScrollRef = ref(null);
const isDarkBg = ref(false);

// MODAL & FOOTER FORM STATE
const isModalOpen = ref(false);
const currentStep = ref(1);
const form = ref({
    service: 'Web Development',
    timeline: '1-3 months',
    contactMethod: 'whatsapp',
    name: '',
    email: '',
    details: ''
});

const servicesList = [
    { name: 'Web Development', icon: '<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9" /></svg>' },
    { name: 'Mobile App Dev', icon: '<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 18h.01M8 21h8a2 2 0 002-2V5a2 2 0 00-2-2H8a2 2 0 00-2 2v14a2 2 0 002 2z" /></svg>' },
    { name: 'UI/UX Design', icon: '<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 21a4 4 0 01-4-4V5a2 2 0 012-2h4a2 2 0 012 2v12a4 4 0 01-4 4zm0 0h12a2 2 0 002-2v-4a2 2 0 00-2-2h-2.343M11 7.343l1.657-1.657a2 2 0 012.828 0l2.829 2.829a2 2 0 010 2.828l-8.486 8.485M7 17h.01" /></svg>' },
    { name: 'Full Stack Solution', icon: '<svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" /></svg>' }
];

const timelineList = ['< 1 month', '1-3 months', '> 3 months'];

const openModal = () => { isModalOpen.value = true; currentStep.value = 1; };
const closeModal = () => { isModalOpen.value = false; };
const nextStep = () => { if (currentStep.value < 3) currentStep.value++; };
const prevStep = () => { if (currentStep.value > 1) currentStep.value--; };

const submitForm = () => {
    const msg = `Hello Nipun! I'm interested in starting a project.\n\n*Service:* ${form.value.service}\n*Timeline:* ${form.value.timeline}\n*Name:* ${form.value.name || 'Not Provided'}\n*Email:* ${form.value.email || 'Not Provided'}\n*Details:* ${form.value.details || 'N/A'}`;
    if (form.value.contactMethod === 'whatsapp') {
        window.open(`https://wa.me/94710474475?text=${encodeURIComponent(msg)}`, '_blank');
    } else {
        window.open(`mailto:sudarakanipun07@gmail.com?subject=New Project Inquiry - ${form.value.service}&body=${encodeURIComponent(msg)}`, '_blank');
    }
    closeModal();
};

const submitFooterForm = () => {
    const msg = `Hello Nipun!\n\n*Name:* ${form.value.name || 'Not Provided'}\n*Email:* ${form.value.email || 'Not Provided'}\n*Message:* ${form.value.details || 'N/A'}`;
    window.open(`mailto:sudarakanipun07@gmail.com?subject=New Contact Message&body=${encodeURIComponent(msg)}`, '_blank');
    form.value.name = '';
    form.value.email = '';
    form.value.details = '';
};

const scrollTo = (id) => {
    const el = document.getElementById(id);
    if (el) {
        el.scrollIntoView({ behavior: 'smooth' });
        isSidebarOpen.value = false;
    }
};

const categories = ['All Projects', 'Academic Project', 'Client Work', 'Mobile & Apps', '3D & Creative'];
const activeCategory = ref('All Projects');

const projects = ref([
    {
        id: 1,
        title: 'Pet Clinic Management',
        description: 'An end-to-end management platform engineered for veterinary operations featuring live queues.',
        image: 'https://images.unsplash.com/photo-1583337130417-3346a1be7dee?q=80&w=800',
        category: 'Academic Project',
        rating: '4.9',
        tools: 'Vue & Laravel',
        link: 'https://pet-connect-production-944c.up.railway.app',
        icons: [
            '<svg viewBox="0 0 24 24" fill="#FF2D20"><path d="M23.3 8.3l-10-5.8c-.8-.5-1.8-.5-2.6 0l-10 5.8c-.8.5-1.3 1.4-1.3 2.3v11.6l5.2-3v-7c0-.2.1-.3.3-.4l6.1-3.5c.2-.1.5-.1.7 0l5.8 3.4v6.8l5.2 3V10.6c0-1-.5-1.8-1.3-2.3z" /></svg>',
            '<svg viewBox="0 0 256 221" fill="#41B883"><path d="M204.8 0H256L128 220.8L0 0H51.2L128 132.48L204.8 0Z" /></svg>'
        ]
    },
    {
        id: 2,
        title: 'Dream Tailors App',
        description: 'Custom tailoring web application with fabric upload management, billing, and professional invoices.',
        image: 'https://images.unsplash.com/photo-1556905055-8f358a7a47b2?q=80&w=800',
        category: 'Academic Project',
        rating: '4.8',
        tools: 'JS & MySQL',
        link: '#',
        icons: [
            '<svg viewBox="0 0 24 24" fill="#F7DF1E"><path d="M0 0h24v24H0V0z" fill="none" /><path d="M6 15.46c-1.04-.3-1.63-.98-1.74-2.02h-2.9c.14 2.29 1.7 3.96 4.63 3.96 2.76 0 4.25-1.4 4.25-3.32 0-2.22-1.68-2.92-4.14-3.5-1.92-.46-2.38-.85-2.38-1.54 0-.75.7-1.3 1.95-1.3 1.34 0 2.05.6 2.14 1.54h2.86c-.1-2.08-1.57-3.52-5-3.52-2.73 0-4.8 1.4-4.8 3.5 0 2.1 1.63 2.78 4.2 3.39 1.83.43 2.34.87 2.34 1.59 0 .86-.82 1.35-2.08 1.35zM15 17.5V3h3v14.5c0 2.82-1.92 4.5-4.85 4.5-2.82 0-4.66-1.54-4.85-3.96h2.95c.1 1.25.96 1.73 1.9 1.73 1.2 0 1.85-.62 1.85-2.27z" /></svg>',
            '<svg viewBox="0 0 24 24" fill="#4479A1"><path d="M12 2.2c-5.5 0-10 4.5-10 10s4.5 10 10 10 10-4.5 10-10-4.5-10-10-10zm4.5 14.5l-2.5-1.5-2.5 1.5.5-2.8-2-1.9 2.8-.4 1.2-2.5 1.2 2.5 2.8.4-2 1.9.5 2.8z" /></svg>'
        ]
    }
]);

const filteredProjects = computed(() => {
    if (activeCategory.value === 'All Projects') {
        return projects.value;
    }
    return projects.value.filter(project => project.category === activeCategory.value);
});

const row1 = [
    { name: 'JavaScript', svg: '<svg fill="#F7DF1E" viewBox="0 0 24 24"><path d="M0 0h24v24H0V0z" fill="none" /><path d="M6 15.46c-1.04-.3-1.63-.98-1.74-2.02h-2.9c.14 2.29 1.7 3.96 4.63 3.96 2.76 0 4.25-1.4 4.25-3.32 0-2.22-1.68-2.92-4.14-3.5-1.92-.46-2.38-.85-2.38-1.54 0-.75.7-1.3 1.95-1.3 1.34 0 2.05.6 2.14 1.54h2.86c-.1-2.08-1.57-3.52-5-3.52-2.73 0-4.8 1.4-4.8 3.5 0 2.1 1.63 2.78 4.2 3.39 1.83.43 2.34.87 2.34 1.59 0 .86-.82 1.35-2.08 1.35zM15 17.5V3h3v14.5c0 2.82-1.92 4.5-4.85 4.5-2.82 0-4.66-1.54-4.85-3.96h2.95c.1 1.25.96 1.73 1.9 1.73 1.2 0 1.85-.62 1.85-2.27z" /></svg>' },
    { name: 'Vue.js', svg: '<svg viewBox="0 0 256 221" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M204.8 0H256L128 220.8L0 0H51.2L128 132.48L204.8 0Z" fill="#41B883" /><path d="M204.8 0H153.6L128 44.16L102.4 0H51.2L128 132.48L204.8 0Z" fill="#34495E" /></svg>' },
    { name: 'Tailwind CSS', svg: '<svg fill="#38BDF8" viewBox="0 0 54 33"><path d="M27 0C21.6 0 18.225 2.7 16.875 8.1 19.575 5.4 22.95 4.725 27 5.4c2.25.375 3.863 2.063 5.625 3.938C35.213 12.188 38.363 15.3 40.5 15.3c5.4 0 8.775-2.7 10.125-8.1-2.7 2.7-6.075 3.375-10.125 2.7-2.25-.375-3.863-2.063-5.625-3.938C32.287 3.113 29.137 0 27 0zM13.5 16.2C8.1 16.2 4.725 18.9 3.375 24.3c2.7-2.7 6.075-3.375 10.125-2.7 2.25.375 3.863 2.063 5.625 3.938C21.713 28.388 24.863 31.5 27 31.5c5.4 0 8.775-2.7 10.125-8.1-2.7 2.7-6.075 3.375-10.125 2.7-2.25-.375-3.863-2.063-5.625-3.938C18.787 19.313 15.637 16.2 13.5 16.2z" /></svg>' },
    { name: 'HTML5', svg: '<svg fill="#E34F26" viewBox="0 0 24 24"><path d="M1.5 0h21l-1.91 21.563L11.977 24l-8.565-2.438L1.5 0zm17.09 4.16l-.3 3.357H8.64l.14 1.583h9.2l-.76 8.523-5.24 1.458-5.24-1.458-.33-3.708h3.38l.17 1.833 2.01.558 2.02-.558.33-3.666H4.8l.58-6.417h13.21z" /></svg>' },
    { name: 'CSS3', svg: '<svg fill="#1572B6" viewBox="0 0 24 24"><path d="M1.5 0h21l-1.91 21.563L11.977 24l-8.564-2.438L1.5 0zm17.09 4.16l-.3 3.357H8.64l.14 1.583h9.2l-.76 8.523-5.24 1.458-5.24-1.458-.33-3.708h3.38l.17 1.833 2.01.558 2.02-.558.33-3.666H4.8l.58-6.417h13.21z" /></svg>' },
];
const row2 = [
    { name: 'PHP', svg: '<svg fill="#777BB4" viewBox="0 0 24 24"><path d="M12 2C5.373 2 0 6.477 0 12s5.373 10 12 10 12-4.477 12-10S18.627 2 12 2zm-1.8 13.5h-1.6l1-5h2.8c1.3 0 2.2.8 2.2 2s-.9 2-2.2 2h-1.2l-.2 1zM20.2 11c0 1.2-.9 2-2.2 2h-1.2l-.5 2.5h-1.6l1.4-7h2.8c1.3 0 2.2.8 2.2 2.5zm-14.7.5h-1.2l-.5 2.5H2.2l1.4-7h2.8c1.3 0 2.2.8 2.2 2s-.9 2.5-2.2 2.5zm11.7-1h-1l-.4 2h1c.5 0 .8-.3.8-.8s-.3-1.2-.8-1.2zm-12.8 0h-1l-.4 2h1c.5 0 .8-.3.8-.8s-.3-1.2-.8-1.2z" /></svg>' },
    { name: 'Laravel', svg: '<svg fill="#FF2D20" viewBox="0 0 24 24"><path d="M23.3 8.3l-10-5.8c-.8-.5-1.8-.5-2.6 0l-10 5.8c-.8.5-1.3 1.4-1.3 2.3v11.6l5.2-3v-7c0-.2.1-.3.3-.4l6.1-3.5c.2-.1.5-.1.7 0l5.8 3.4v6.8l5.2 3V10.6c0-1-.5-1.8-1.3-2.3z" /></svg>' },
    { name: 'MySQL', svg: '<svg fill="#4479A1" viewBox="0 0 24 24"><path d="M12 2.2c-5.5 0-10 4.5-10 10s4.5 10 10 10 10-4.5 10-10-4.5-10-10-10zm4.5 14.5l-2.5-1.5-2.5 1.5.5-2.8-2-1.9 2.8-.4 1.2-2.5 1.2 2.5 2.8.4-2 1.9.5 2.8z" /></svg>' },
    { name: 'Docker', svg: '<svg fill="#2496ED" viewBox="0 0 24 24"><path d="M2.6 13.9l.4 1.5c2 4.1 6.8 6.4 11.2 5 2.5-.8 4.6-2.5 5.8-4.8l-1.3-1c-.9 1.8-2.6 3.1-4.7 3.7-3.4 1-7.1-.6-8.6-3.8l-2.8-.6zm18.3-4c-.2-.6-.8-1-1.5-1h-2.5c-.8 0-1.5-1h-2.5c-.8 0-1.5.7-1.5 1.5S16.1 12 16.9 12h2.5c.8 0 1.5-.7 1.5-1.5s-.1-1-.4-1.5zm-5.7.5H12c-.8 0-1.5.7-1.5 1.5s.7 1.5 1.5 1.5h3.2c.8 0 1.5-.7 1.5-1.5s-.7-1.5-1.5-1.5z" /></svg>' },
    { name: 'GitHub', svg: '<svg fill="#181717" viewBox="0 0 24 24"><path d="M12 .3a12 12 0 00-3.8 23.4c.6.1.8-.3.8-.6v-2.2c-3.3.7-4-1.6-4-1.6-.5-1.4-1.3-1.8-1.3-1.8-1.1-.7.1-.7.1-.7 1.2.1 1.8 1.2 1.8 1.2 1.1 1.8 2.8 1.3 3.5 1 .1-.8.4-1.3.8-1.6-2.7-.3-5.5-1.3-5.5-5.9 0-1.3.5-2.4 1.2-3.2-.1-.3-.5-1.5.1-3.2 0 0 1-.3 3.3 1.2a11.5 11.5 0 016 0c2.3-1.5 3.3-1.2 3.3-1.2.6 1.7.2 2.9.1 3.2.7.8 1.2 1.9 1.2 3.2 0 4.6-2.8 5.6-5.5 5.9.4.4.8 1.1.8 2.2v3.3c0 .3.2.7.8.6A12 12 0 0012 .3z" /></svg>' },
];
const row3 = [
    { name: 'MongoDB', svg: '<svg fill="#47A248" viewBox="0 0 24 24"><path d="M11.6 1.1A10.3 10.3 0 007 10.5c0 3.7 2 6.8 4.6 8.5v3.9s.4.1.6 0v-4c2.5-1.6 4.4-4.8 4.4-8.4 0-4-3-8-5-9.4z" /></svg>' },
    { name: 'Linux', svg: '<svg fill="#FCC624" viewBox="0 0 24 24"><path d="M12 2C8 2 5 7 5 13s2.5 9 7 9 7-3 7-9-3-11-7-11z" /></svg>' },
    { name: 'Adobe XD', svg: '<svg fill="#FF61F6" viewBox="0 0 24 24"><path d="M5 2h14c1.7 0 3 1.3 3 3v14c0 1.7-1.3 3-3 3H5c-1.7 0-3-1.3-3-3V5c0-1.7 1.3-3 3-3zm4.5 12.8L6.2 9h2.3l2 3.5 2-3.5h2.3l-3.3 5.8 3.4 5.7H13l-2.2-4-2.2 4H6.2l3.3-5.7zm4.3 2.2h2.5c2.3 0 4.2-1.8 4.2-4.2S18.1 8.6 15.8 8.6h-2v8.4zm2-6.5h.5c1.2 0 2.2 1 2.2 2.3s-1 2.3-2.2 2.3h-.5v-4.6z" /></svg>' },
    { name: 'Three.js', svg: '<svg fill="#000000" viewBox="0 0 24 24"><path d="M12 2.1L2.3 7.7l9.7 5.6 9.7-5.6L12 2.1zm0 12.8l-8.5-4.9v9.8l8.5 4.9V14.9zm1.2 0v9.8l8.5-4.9V10l-8.5 4.9z" /></svg>' },
    { name: 'Unreal Engine', svg: '<svg fill="#313131" viewBox="0 0 24 24"><path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm3.8 14H13v-5c0-1.1-.9-2-2-2s-2 .9-2 2v5H6.2V9.8h2.8v1.3c.6-.8 1.6-1.3 2.7-1.3 2.3 0 4.1 1.9 4.1 4.2V16z" /></svg>' },
];

const isAtAboutSection = computed(() => {
    return scrollY.value > window.innerHeight * 0.8 && scrollY.value < window.innerHeight * 2.2;
});

const scrollProjects = (direction) => {
    if (projectsScrollRef.value) {
        const scrollAmount = window.innerWidth < 768 ? 280 : 350; 
        const currentScroll = projectsScrollRef.value.scrollLeft;
        projectsScrollRef.value.scrollTo({
            left: direction === 'left' ? currentScroll - scrollAmount : currentScroll + scrollAmount,
            behavior: 'smooth'
        });
    }
};

const getStartPos = () => {
    const angle = Math.random() * Math.PI * 2;
    const radius = 1000 + Math.random() * 800; 
    return { x: Math.cos(angle) * radius, y: Math.sin(angle) * radius };
};

const createLetters = (str, prefix) => {
    return str.split('').map((char, i) => ({
        id: `${prefix}_${i}`,
        char,
        x: getStartPos().x,
        y: getStartPos().y,
        isHovered: false,
        isConfused: false,
        isHappy: false
    }));
};

const correctLine1 = createLetters("I'M A FULL", 'l1');
const correctLine2 = createLetters("STACK DEVELOPER", 'l2');
const correctLeftLine1 = createLetters("WEB/SYSTEM", 'll1');
const correctLeftLine2 = createLetters("DESIGNER", 'll2');
const correctRightLine1 = createLetters("NIPUN", 'r1');
const correctRightLine2 = createLetters("SUDARAKA", 'r2');

const scrambleLetters = (correctArray) => {
    const arr = [...correctArray];
    const nonSpace = arr.filter(l => l.char !== ' ');
    for (let i = nonSpace.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [nonSpace[i], nonSpace[j]] = [nonSpace[j], nonSpace[i]];
    }
    let nsIdx = 0;
    return arr.map(l => l.char === ' ' ? l : nonSpace[nsIdx++]);
};

const line1 = ref([...correctLine1]);
const line2 = ref([...correctLine2]);
const leftLine1 = ref([...correctLeftLine1]);
const leftLine2 = ref([...correctLeftLine2]);
const rightLine1 = ref([...correctRightLine1]);
const rightLine2 = ref([...correctRightLine2]);

const timelineActive = ref([false, false, false]);

const handleScroll = () => {
    scrollY.value = window.scrollY;
    isScrolled.value = window.scrollY > 100;

    const y = window.scrollY;
    const h = window.innerHeight;
    const contactEl = document.getElementById('contact');
    const contactTop = contactEl ? contactEl.offsetTop - h / 3 : 9999999;
    
    isDarkBg.value = (y > h * 0.8 && y < h * 2.2) || (y > contactTop);

    const items = document.querySelectorAll('.timeline-item');
    const triggerPoint = window.innerHeight * 0.75; 
    items.forEach((item, index) => {
        const top = item.getBoundingClientRect().top;
        if (top < triggerPoint) {
            timelineActive.value[index] = true; 
        } else {
            timelineActive.value[index] = false; 
        }
    });
};

onMounted(() => {
    line1.value = scrambleLetters(correctLine1);
    line2.value = scrambleLetters(correctLine2);
    leftLine1.value = scrambleLetters(correctLeftLine1);
    leftLine2.value = scrambleLetters(correctLeftLine2);
    rightLine1.value = scrambleLetters(correctRightLine1);
    rightLine2.value = scrambleLetters(correctRightLine2);

    setTimeout(() => {
        const resetPos = (arr) => arr.value.forEach(l => { l.x = 0; l.y = 0; });
        resetPos(line1); resetPos(line2); resetPos(leftLine1); resetPos(leftLine2); resetPos(rightLine1); resetPos(rightLine2);
    }, 100);

    setTimeout(() => {
        const confuse = (arr) => arr.value.forEach(l => { if (l.char !== ' ') l.isConfused = true; });
        confuse(line1); confuse(line2); confuse(leftLine1); confuse(leftLine2); confuse(rightLine1); confuse(rightLine2);
    }, 2500);

    setTimeout(() => {
        const unConfuse = (arr) => arr.value.forEach(l => l.isConfused = false);
        unConfuse(line1); unConfuse(line2); unConfuse(leftLine1); unConfuse(leftLine2); unConfuse(rightLine1); unConfuse(rightLine2);

        line1.value = [...correctLine1];
        line2.value = [...correctLine2];
        leftLine1.value = [...correctLeftLine1];
        leftLine2.value = [...correctLeftLine2];
        rightLine1.value = [...correctRightLine1];
        rightLine2.value = [...correctRightLine2];
    }, 4500);

    setTimeout(() => {
        const allLetters = [...line1.value, ...line2.value, ...leftLine1.value, ...leftLine2.value, ...rightLine1.value, ...rightLine2.value];
        allLetters.forEach((l, index) => {
            if (l.char !== ' ') {
                setTimeout(() => {
                    l.isHappy = true;
                    setTimeout(() => l.isHappy = false, 600);
                }, (index % 20) * 80); 
            }
        });
    }, 9500); 

    const observerOptions = { threshold: 0.1, rootMargin: "0px 0px -50px 0px" };
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.remove('opacity-0', 'translate-y-8', 'translate-y-12', 'translate-x-8');
                entry.target.classList.add('opacity-100', 'translate-y-0', 'translate-x-0');
                observer.unobserve(entry.target); 
            }
        });
    }, observerOptions);

    document.querySelectorAll('.scroll-animate, .scroll-animate-img, .vertical-quote').forEach((el) => {
        observer.observe(el);
    });

    window.addEventListener('scroll', handleScroll);
    handleScroll();
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});
</script>

<style>
/* CSS for Smooth Scrolling on entire document */
html {
    scroll-behavior: smooth;
}

/* MODAL ANIMATION */
.modal-fade-enter-active, .modal-fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.modal-fade-enter-from, .modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

/* PROJECT LIST ANIMATIONS */
.project-list-enter-active, .project-list-leave-active {
  transition: all 0.5s ease;
}
.project-list-enter-from, .project-list-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

/* FADE IN CONTENT */
@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}
.animate-fade-in {
    animation: fadeIn 0.4s ease-out forwards;
}

/* MARQUEE ANIMATIONS FOR TECHNOLOGIES SECTION */
@keyframes scroll-left {
    from { transform: translateX(0); }
    to { transform: translateX(-50%); }
}
@keyframes scroll-right {
    from { transform: translateX(-50%); }
    to { transform: translateX(0); }
}
.animate-scroll-left {
    animation: scroll-left 30s linear infinite;
}
.animate-scroll-right {
    animation: scroll-right 30s linear infinite;
}

/* HIDE SCROLLBAR */
.scrollbar-hide::-webkit-scrollbar {
    display: none;
}
.scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
}

/* VERTICAL HOLLOW QUOTE STYLING */
.vertical-quote {
    writing-mode: vertical-rl;
    -webkit-text-stroke: 1px rgba(255, 255, 255, 0.4);
    color: transparent;
    text-transform: uppercase;
}
@media (min-width: 768px) {
    .vertical-quote {
        -webkit-text-stroke: 1.5px rgba(255, 255, 255, 0.5);
    }
}

:root {
    --nav-string-height: 40px;
}
@media (min-width: 768px) {
    :root {
        --nav-string-height: 55px;
    }
}

/* Bounce Ease */
.ease-bounce { transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1); }

/* Wavy "Click Here" Animations */
@keyframes wave-float { 0%, 100% { transform: translateY(0) rotate(-5deg); } 50% { transform: translateY(-10px) rotate(5deg); } }
.animate-wave-float { animation: wave-float 3s ease-in-out infinite; }

/* The Arrow Flow Animation */
@keyframes arrow-flow {
    from { stroke-dashoffset: 12; }
    to { stroke-dashoffset: 0; }
}
.animate-arrow-flow { animation: arrow-flow 0.8s linear infinite; }

/* Cloud Lines Flow Animation */
@keyframes flow-dash { to { stroke-dashoffset: -20; } }
.animate-flow-dash { animation: flow-dash 1s linear infinite; }

/* HANGING NAVBAR SWING ANIMATION */
@keyframes swing { 0% { transform: rotate(4deg); } 100% { transform: rotate(-4deg); } }
.animate-swing { animation: swing infinite alternate ease-in-out; }

/* CV Button Fade In */
@keyframes fade-in-up {
    0% { opacity: 0; transform: translateY(10px); }
    100% { opacity: 1; transform: translateY(0); }
}
.anim-fade-in-cv { animation: fade-in-up 0.6s ease-out forwards; animation-delay: 5s; }

/* Straight Line Scale Animations */
@keyframes draw-line-scale { 0% { transform: scaleX(0); } 100% { transform: scaleX(1); } }
.animate-line-left { transform-origin: right center; animation: draw-line-scale 2s cubic-bezier(0.4, 0, 0.2, 1) forwards; animation-delay: 4.8s; transform: scaleX(0); }
.animate-line-right { transform-origin: left center; animation: draw-line-scale 2s cubic-bezier(0.4, 0, 0.2, 1) forwards; animation-delay: 4.8s; transform: scaleX(0); }

/* 3D Hollow text effect */
.letter-front { -webkit-text-stroke: 2px #EBE5D9; color: transparent; }
.letter-front.is-active { -webkit-text-stroke: 2px #FF6B00 !important; }

/* Organic Fly & Slow Sort */
.letter-fly { transition: transform 1.8s cubic-bezier(0.2, 0.8, 0.2, 1); }
.slow-sort-move { transition: transform 4.5s ease-in-out; }

/* Letter Wiggle */
@keyframes wiggle-infinite { 0%, 100% { transform: rotate(-8deg); } 50% { transform: rotate(8deg); } }
.animate-wiggle-infinite { animation: wiggle-infinite 0.3s ease-in-out infinite; }

@keyframes look-confused { 0%, 100% { transform: translateX(0); } 25% { transform: translateX(-3px) rotate(-3deg); } 75% { transform: translateX(3px) rotate(3deg); } }
.animate-confused { animation: look-confused 0.2s ease-in-out infinite; }

@keyframes happy-jump { 0%, 100% { transform: translateY(0) scale(1); } 50% { transform: translateY(-25px) scale(1.2); } }
.animate-happy-jump { animation: happy-jump 0.5s cubic-bezier(0.34, 1.56, 0.64, 1); }

/* Scroll badge spin */
@keyframes spin-slow { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
.animate-spin-slow { animation: spin-slow 8s linear infinite; }
</style>