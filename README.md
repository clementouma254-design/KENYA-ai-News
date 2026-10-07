# KENYA-ai-News

```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pulse AI News - Smart AI News Aggregator</title>
  
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#eef2ff',
              100: '#e0e7ff',
              500: '#6366f1',
              600: '#4f46e5',
              700: '#4338ca',
              900: '#312e81',
            },
            accent: {
              cyan: '#06b6d4',
              emerald: '#10b981',
              rose: '#f43f5e',
              amber: '#f59e0b'
            }
          },
          animation: {
            'pulse-slow': 'pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite',
            'bounce-gentle': 'bounce 2s infinite',
            'wave': 'wave 1.2s ease-in-out infinite'
          },
          keyframes: {
            wave: {
              '0%, 100%': { height: '6px' },
              '50%': { height: '20px' }
            }
          }
        }
      }
    }
  </script>

  <!-- Google Fonts: Inter & JetBrains Mono -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
  
  <!-- Font Awesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

  <style>
    body {
      font-family: 'Inter', sans-serif;
      transition: background-color 0.3s ease, color 0.3s ease;
    }
    
    /* Custom Scrollbar */
    ::-webkit-scrollbar {
      width: 6px;
      height: 6px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(15, 23, 42, 0.6);
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(99, 102, 241, 0.5);
      border-radius: 9999px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: rgba(99, 102, 241, 0.8);
    }

    /* Glassmorphism Surface */
    .glass-card {
      background: rgba(30, 41, 59, 0.7);
      backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }
    .light .glass-card {
      background: rgba(255, 255, 255, 0.85);
      backdrop-filter: blur(12px);
      border: 1px solid rgba(0, 0, 0, 0.08);
    }

    .ad-pattern {
      background-image: radial-gradient(rgba(99, 102, 241, 0.15) 1px, transparent 1px);
      background-size: 12px 12px;
    }
  </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col selection:bg-brand-500 selection:text-white transition-colors duration-200">

  <!-- Top Navigation Header -->
  <header class="sticky top-0 z-40 bg-slate-900/90 dark:bg-slate-950/90 backdrop-blur-md border-b border-slate-800/80 transition-colors">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between gap-4">
      
      <!-- Brand Logo -->
      <div class="flex items-center gap-3 cursor-pointer" onclick="setActiveCategory('All')">
        <div class="relative flex items-center justify-center w-10 h-10 rounded-xl bg-gradient-to-tr from-brand-600 via-indigo-500 to-accent-cyan shadow-lg shadow-brand-500/20">
          <i class="fa-solid fa-bolt text-white text-lg animate-pulse-slow"></i>
          <span class="absolute -top-1 -right-1 flex h-3 w-3">
            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-accent-cyan opacity-75"></span>
            <span class="relative inline-flex rounded-full h-3 w-3 bg-accent-cyan"></span>
          </span>
        </div>
        <div>
          <div class="flex items-center gap-2">
            <span class="font-extrabold text-xl tracking-tight bg-gradient-to-r from-white via-slate-200 to-indigo-300 bg-clip-text text-transparent dark:from-white dark:to-indigo-200">
              PULSE<span class="text-brand-500">.AI</span>
            </span>
            <span class="px-2 py-0.5 text-[10px] font-bold rounded-full bg-brand-500/10 text-brand-400 border border-brand-500/30 uppercase tracking-widest">
              Live
            </span>
          </div>
          <p class="text-[11px] text-slate-400 hidden sm:block">AI-Summarized Real-Time Intelligence</p>
        </div>
      </div>

      <!-- Search Bar -->
      <div class="flex-1 max-w-md relative hidden md:block">
        <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
        <input 
          type="text" 
          id="searchInput" 
          placeholder="Search news, topics, or sources..." 
          oninput="handleSearch(this.value)"
          class="w-full bg-slate-900/90 dark:bg-slate-900 border border-slate-800 rounded-full pl-10 pr-10 py-2 text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:border-brand-500 focus:ring-1 focus:ring-brand-500 transition-all"
        />
        <button id="clearSearchBtn" onclick="clearSearch()" class="hidden absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 hover:text-slate-300">
          <i class="fa-solid fa-xmark text-xs"></i>
        </button>
      </div>

      <!-- Header Action Buttons -->
      <div class="flex items-center gap-2 sm:gap-3">
        
        <!-- Mobile Search Toggle Button -->
        <button onclick="toggleMobileSearch()" class="md:hidden p-2 rounded-xl text-slate-400 hover:text-slate-200 hover:bg-slate-800/60 transition-all" title="Search">
          <i class="fa-solid fa-magnifying-glass text-lg"></i>
        </button>

        <!-- Custom AI Summarizer Button -->
        <button onclick="openSummarizerModal()" class="flex items-center gap-2 px-3 py-2 rounded-xl bg-gradient-to-r from-brand-600/20 to-accent-cyan/20 border border-brand-500/40 text-brand-300 hover:text-white hover:bg-brand-600/30 text-xs sm:text-sm font-semibold transition-all shadow-sm">
          <i class="fa-solid fa-wand-magic-sparkles text-accent-cyan"></i>
          <span class="hidden sm:inline">Custom AI Summarizer</span>
          <span class="sm:hidden">AI Tool</span>
        </button>

        <!-- Premium Status Toggle / Trigger -->
        <button id="headerPremiumBtn" onclick="openPremiumModal()" class="flex items-center gap-1.5 px-3 py-2 rounded-xl bg-gradient-to-r from-amber-500 to-yellow-500 hover:from-amber-600 hover:to-yellow-600 text-slate-950 font-bold text-xs sm:text-sm transition-all shadow-md shadow-amber-500/20">
          <i class="fa-solid fa-crown text-slate-950"></i>
          <span id="premiumBtnText">Go Premium</span>
        </button>

        <!-- Theme Toggle -->
        <button onclick="toggleTheme()" class="p-2.5 rounded-xl bg-slate-900 border border-slate-800 text-slate-400 hover:text-amber-400 transition-all" title="Toggle Dark/Light Mode">
          <i id="themeIcon" class="fa-solid fa-moon text-base"></i>
        </button>
      </div>
    </div>

    <!-- Mobile Search Bar Drawer -->
    <div id="mobileSearchDrawer" class="hidden md:hidden px-4 py-2 bg-slate-900 border-b border-slate-800">
      <div class="relative">
        <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
        <input 
          type="text" 
          id="mobileSearchInput" 
          placeholder="Search news, topics, or sources..." 
          oninput="handleSearch(this.value)"
          class="w-full bg-slate-950 border border-slate-800 rounded-xl pl-10 pr-10 py-2 text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:border-brand-500"
        />
      </div>
    </div>
  </header>

  <!-- Main Content Wrapper -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">

    <!-- Categories & Filtering Ribbon -->
    <div class="sticky top-16 z-30 py-2 -mx-4 px-4 sm:-mx-6 sm:px-6 lg:-mx-8 lg:px-8 bg-slate-950/80 backdrop-blur-md border-b border-slate-800/50">
      <div class="flex items-center justify-between gap-4 overflow-x-auto no-scrollbar">
        <div class="flex items-center gap-2 py-1 flex-nowrap" id="categoryContainer">
          <!-- Categories rendered via JS -->
        </div>

        <!-- Quick Filter Meta Info -->
        <div class="flex items-center gap-3 shrink-0 text-xs text-slate-400">
          <button onclick="toggleBookmarkFilter()" id="bookmarkFilterBtn" class="flex items-center gap-1.5 px-3 py-1.5 rounded-lg border border-slate-800 bg-slate-900 hover:bg-slate-800 transition-all">
            <i class="fa-solid fa-bookmark text-amber-400"></i>
            <span>Saved (<span id="bookmarkCount">0</span>)</span>
          </button>
        </div>
      </div>
    </div>

    <!-- Hero Announcement / Ad Banner Simulator Alert -->
    <div id="adNoticeBanner" class="p-4 rounded-2xl bg-gradient-to-r from-brand-900/40 via-slate-900 to-indigo-950/50 border border-brand-500/20 flex flex-col md:flex-row items-start md:items-center justify-between gap-4 relative overflow-hidden">
      <div class="flex items-start gap-3.5 z-10">
        <div class="p-2.5 rounded-xl bg-brand-500/20 text-brand-400 border border-brand-500/30 shrink-0 mt-0.5 md:mt-0">
          <i class="fa-solid fa-sparkles text-lg"></i>
        </div>
        <div>
          <h3 class="text-sm font-bold text-slate-100 flex items-center gap-2">
            AI-Powered Smart News Summarizer Active
            <span class="px-2 py-0.5 text-[10px] rounded-md bg-accent-emerald/20 text-accent-emerald font-mono">Gemini 3 Flash</span>
          </h3>
          <p class="text-xs text-slate-400 mt-0.5">Instant 3-bullet key insights, 1-sentence TL;DRs, sentiment breakdown & text-to-speech engine on all stories.</p>
        </div>
      </div>
      <div class="flex items-center gap-3 z-10 self-end md:self-auto shrink-0">
        <button onclick="openPremiumModal()" class="px-3.5 py-1.5 rounded-xl bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold text-xs transition-all shadow-sm">
          Remove Ads & Unlock Pro
        </button>
      </div>
    </div>

    <!-- Layout Grid: Main News Feed + Sidebar -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
      
      <!-- Left Column: Articles List (8 cols on desktop) -->
      <section class="lg:col-span-8 space-y-6" id="newsFeedSection">
        
        <!-- Active Filter Indicator -->
        <div class="flex items-center justify-between text-xs text-slate-400 px-1">
          <div class="flex items-center gap-2">
            <span class="font-semibold text-slate-200 text-sm" id="currentFeedTitle">Top Headlines</span>
            <span class="px-2 py-0.5 rounded-full bg-slate-800 text-slate-400" id="articleCountBadge">0 articles</span>
          </div>
          <div class="flex items-center gap-2">
            <span>Sort by:</span>
            <select id="sortSelect" onchange="handleSortChange(this.value)" class="bg-slate-900 border border-slate-800 rounded-lg text-xs text-slate-300 px-2 py-1 focus:outline-none focus:border-brand-500">
              <option value="latest">Latest First</option>
              <option value="popular">Most Read</option>
              <option value="positive">Positive Sentiment</option>
              <option value="critical">Critical First</option>
            </select>
          </div>
        </div>

        <!-- Dynamic News Feed Container -->
        <div id="articlesList" class="space-y-6">
          <!-- Articles injected via JavaScript -->
        </div>

        <!-- Empty State Container -->
        <div id="emptyState" class="hidden py-16 text-center space-y-3 glass-card rounded-2xl">
          <i class="fa-solid fa-newspaper text-4xl text-slate-600"></i>
          <h3 class="text-base font-semibold text-slate-300">No news stories found</h3>
          <p class="text-xs text-slate-500 max-w-sm mx-auto">Try adjusting your search terms or clearing category filters to view more articles.</p>
          <button onclick="resetFilters()" class="px-4 py-2 rounded-xl bg-brand-600 text-white text-xs font-semibold hover:bg-brand-500 transition-all">
            Reset Filters
          </button>
        </div>
      </section>

      <!-- Right Column: Sidebar (4 cols on desktop) -->
      <aside class="lg:col-span-4 space-y-6 sticky top-32">
        
        <!-- Quick Custom AI Summarizer Mini Card -->
        <div class="glass-card p-5 rounded-2xl relative overflow-hidden border border-slate-800">
          <div class="flex items-center justify-between mb-3">
            <div class="flex items-center gap-2 text-brand-400 font-bold text-xs uppercase tracking-wider">
              <i class="fa-solid fa-bolt text-accent-cyan"></i>
              Instant AI Analysis
            </div>
            <span class="px-2 py-0.5 text-[10px] rounded bg-brand-500/10 text-brand-300 border border-brand-500/20 font-mono">Free Tool</span>
          </div>
          <h4 class="text-sm font-bold text-slate-100 mb-1">Summarize Any Article or Link</h4>
          <p class="text-xs text-slate-400 mb-3">Paste any news link or block of text below to extract 3 bullet points & TL;DR powered by Gemini 3 Flash.</p>
          
          <button onclick="openSummarizerModal()" class="w-full py-2.5 rounded-xl bg-gradient-to-r from-brand-600 to-indigo-600 hover:from-brand-500 hover:to-indigo-500 text-white font-semibold text-xs transition-all shadow-md shadow-brand-500/20 flex items-center justify-center gap-2">
            <i class="fa-solid fa-wand-magic-sparkles"></i>
            Launch Custom AI Summarizer
          </button>
        </div>

        <!-- Live Trending Topics -->
        <div class="glass-card p-5 rounded-2xl border border-slate-800">
          <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-4 flex items-center gap-2">
            <i class="fa-solid fa-fire text-accent-rose"></i>
            Trending News Topics
          </h3>
          <div class="flex flex-wrap gap-2" id="trendingTopicsContainer">
            <!-- Rendered dynamically -->
          </div>
        </div>

        <!-- App Monetization Simulator Metrics (Educational / Dashboard Widget) -->
        <div class="glass-card p-5 rounded-2xl border border-slate-800 space-y-3">
          <div class="flex items-center justify-between">
            <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 flex items-center gap-2">
              <i class="fa-solid fa-chart-line text-accent-emerald"></i>
              AdMob Live Ad Stats
            </h3>
            <span id="adStatusBadge" class="px-2 py-0.5 text-[10px] font-semibold rounded bg-accent-emerald/20 text-accent-emerald">
              Ads Active
            </span>
          </div>
          <p class="text-xs text-slate-400">Simulated publisher revenue metrics from inline MREC units & bottom banners.</p>
          
          <div class="grid grid-cols-2 gap-3 pt-2">
            <div class="p-3 rounded-xl bg-slate-900/80 border border-slate-800/80">
              <span class="text-[10px] text-slate-500 uppercase tracking-wider block font-medium">Ad Impressions</span>
              <span class="text-lg font-mono font-bold text-slate-200" id="adImpressionsCount">24</span>
            </div>
            <div class="p-3 rounded-xl bg-slate-900/80 border border-slate-800/80">
              <span class="text-[10px] text-slate-500 uppercase tracking-wider block font-medium">Est. Revenue ($)</span>
              <span class="text-lg font-mono font-bold text-accent-emerald" id="adEarningsAmount">$0.18</span>
            </div>
          </div>

          <button onclick="openPremiumModal()" class="w-full text-center text-xs text-brand-400 hover:text-brand-300 font-semibold pt-1 transition-colors">
            Simulate Premium ($1.99/mo) to Disable Ads &rarr;
          </button>
        </div>

      </aside>

    </div>
  </main>

  <!-- Custom AI Summarizer Modal -->
  <div id="summarizerModal" class="fixed inset-0 z-50 hidden bg-slate-950/80 backdrop-blur-md flex items-center justify-center p-4">
    <div class="bg-slate-900 border border-slate-800 w-full max-w-2xl rounded-2xl shadow-2xl overflow-hidden flex flex-col max-h-[90vh]">
      
      <!-- Modal Header -->
      <div class="px-6 py-4 border-b border-slate-800 flex items-center justify-between bg-slate-950/50">
        <div class="flex items-center gap-2.5">
          <div class="p-2 rounded-xl bg-brand-500/20 text-brand-400">
            <i class="fa-solid fa-wand-magic-sparkles text-lg"></i>
          </div>
          <div>
            <h3 class="font-bold text-slate-100 text-base">Gemini Custom AI Summarizer</h3>
            <p class="text-xs text-slate-400">Extract 3 bullet summaries & TL;DRs from raw text or news link</p>
          </div>
        </div>
        <button onclick="closeSummarizerModal()" class="p-2 text-slate-400 hover:text-white rounded-lg hover:bg-slate-800 transition-all">
          <i class="fa-solid fa-xmark text-lg"></i>
        </button>
      </div>

      <!-- Modal Body -->
      <div class="p-6 overflow-y-auto space-y-4 flex-1">
        
        <div>
          <label class="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-2">
            Paste News Article Text or Article URL
          </label>
          <textarea 
            id="aiInputText" 
            rows="6" 
            placeholder="Paste raw news text, press release, or article narrative here..."
            class="w-full bg-slate-950 border border-slate-800 rounded-xl p-3.5 text-sm text-slate-200 placeholder-slate-600 focus:outline-none focus:border-brand-500 focus:ring-1 focus:ring-brand-500 font-sans"
          ></textarea>
        </div>

        <div class="flex flex-wrap items-center justify-between gap-3">
          <div class="flex items-center gap-2">
            <button onclick="loadSampleText()" class="px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-xs text-slate-300 transition-all">
              <i class="fa-solid fa-file-lines text-slate-400 mr-1"></i>
              Load Sample Text
            </button>
            <button onclick="clearSummarizerInput()" class="px-3 py-1.5 rounded-lg bg-slate-800 hover:bg-slate-700 text-xs text-slate-300 transition-all">
              Clear
            </button>
          </div>

          <button 
            id="runAiSummarizeBtn"
            onclick="runGeminiSummarize()" 
            class="px-5 py-2.5 rounded-xl bg-gradient-to-r from-brand-600 via-indigo-600 to-accent-cyan hover:from-brand-500 hover:to-accent-cyan text-white font-bold text-xs sm:text-sm shadow-lg shadow-brand-500/25 flex items-center gap-2 transition-all"
          >
            <i class="fa-solid fa-sparkles"></i>
            <span>Generate AI Summary</span>
          </button>
        </div>

        <!-- Output Container -->
        <div id="aiSummaryResultContainer" class="hidden mt-6 pt-6 border-t border-slate-800 space-y-4">
          <div class="flex items-center justify-between">
            <span class="text-xs font-bold text-slate-300 uppercase tracking-wider flex items-center gap-2">
              <i class="fa-solid fa-brain text-brand-400"></i>
              Gemini AI Generated Key Takeaways
            </span>
            <span id="aiSentimentBadge" class="px-2.5 py-0.5 rounded-full text-[10px] font-bold uppercase tracking-wider bg-accent-emerald/20 text-accent-emerald border border-accent-emerald/30">
              Positive
            </span>
          </div>

          <!-- TLDR Card -->
          <div class="p-3.5 rounded-xl bg-brand-500/10 border border-brand-500/20">
            <span class="text-[10px] font-extrabold uppercase text-brand-400 tracking-widest block mb-1">1-Sentence TL;DR</span>
            <p id="aiResultTldr" class="text-xs sm:text-sm font-semibold tex