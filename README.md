# online-food-ordering-website-minigame
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>TableBite Restaurant | Fresh Table Dine-In</title>

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Outfit:wght@500;600;700;800&display=swap" rel="stylesheet">

  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            display: ['Outfit', 'sans-serif'],
            sans: ['"Plus Jakarta Sans"', 'sans-serif'],
          },
          colors: {
            bistro: {
              50: '#f8fafc',
              100: '#f1f5f9',
              200: '#e2e8f0',
              300: '#cbd5e1',
              700: '#334155',
              800: '#1e293b',
              900: '#0f172a',
            },
            terracotta: {
              50: '#fff7ed',
              100: '#ffedd5',
              500: '#ea580c',
              600: '#c2410c',
              700: '#9a3412',
            },
            sage: {
              50: '#f0fdf4',
              100: '#dcfce7',
              600: '#16a34a',
              700: '#15803d',
            }
          }
        }
      }
    }
  </script>
  <style>
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="bg-[#f4efe6] text-bistro-800 font-sans min-h-screen flex flex-col selection:bg-terracotta-500 selection:text-white">

  <!-- Top Navigation Header -->
  <header class="sticky top-0 z-40 bg-white/95 backdrop-blur-md border-b border-stone-200 shadow-sm">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 h-18 py-3 flex items-center justify-between gap-4">
      
      <!-- Brand & Table Picker -->
      <div class="flex items-center gap-3">
        <a href="javascript:void(0)" onclick="switchTab('menu')" class="flex items-center gap-2.5 group">
          <div class="w-10 h-10 rounded-2xl bg-gradient-to-br from-terracotta-500 to-amber-500 flex items-center justify-center text-white text-base font-display font-extrabold shadow-sm group-hover:scale-105 transition">
            TB
          </div>
          <div>
            <div class="font-display font-extrabold text-xl tracking-tight text-bistro-900 flex items-center gap-0.5">
              TableBite<span class="text-terracotta-500 text-2xl leading-none">.</span>
            </div>
            <p class="text-[10px] tracking-wider text-stone-500 font-semibold uppercase -mt-0.5">Kitchen & Bistro</p>
          </div>
        </a>

        <div class="hidden md:flex items-center gap-2 pl-4 border-l border-stone-200">
          <label for="header-table-select" class="text-xs text-stone-500 font-medium">Table Spot:</label>
          <select id="header-table-select" onchange="changeTable(this.value)" class="bg-stone-50 border border-stone-300 text-terracotta-700 text-xs font-bold rounded-xl px-2.5 py-1.5 focus:outline-none focus:ring-2 focus:ring-terracotta-500">
          </select>
        </div>
      </div>

      <!-- Navigation Tabs -->
      <nav class="hidden lg:flex items-center gap-1 bg-stone-100 p-1.5 rounded-2xl border border-stone-200">
        <button onclick="switchTab('menu')" id="btn-tab-menu" class="px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-white bg-terracotta-600 shadow-xs transition">
          Menu
        </button>
        <button onclick="switchTab('favs')" id="btn-tab-favs" class="px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-stone-600 hover:text-stone-900 transition">
          Favorites <span id="fav-counter-badge" class="ml-1 text-[10px] bg-stone-200 text-stone-700 px-1.5 py-0.5 rounded-full hidden">0</span>
        </button>
        <button onclick="switchTab('orders')" id="btn-tab-orders" class="px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-stone-600 hover:text-stone-900 transition">
          Table Orders
        </button>
        <button onclick="switchTab('kds')" id="btn-tab-kds" class="px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-stone-700 hover:text-stone-900 border border-dashed border-stone-300 transition">
          Staff KDS
        </button>
        <button onclick="openCyberGame()" class="px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-cyan-900 bg-cyan-100 hover:bg-cyan-200 border border-cyan-300 transition flex items-center gap-1">
          <span>⚡</span> Cyber Line
        </button>
      </nav>

      <!-- Cart Drawer Trigger & Mobile Hamburger -->
      <div class="flex items-center gap-2.5">
        <button onclick="toggleCart(true)" class="relative flex items-center gap-2.5 px-4 py-2.5 bg-bistro-900 hover:bg-black text-white text-xs font-bold rounded-2xl shadow-sm transition active:scale-95">
          <span class="text-sm">🛒</span>
          <span>Cart</span>
          <span id="cart-item-badge" class="bg-terracotta-500 text-white text-[11px] px-2 py-0.2 rounded-full font-bold">0</span>
          <span id="cart-price-pill" class="pl-1 border-l border-stone-700 text-amber-300 font-mono">₹0</span>
        </button>

        <button onclick="toggleMobileNav()" class="lg:hidden p-2 rounded-xl bg-stone-100 border border-stone-200 text-stone-700">
          ☰
        </button>
      </div>
    </div>

    <!-- Mobile Drawer Navigation -->
    <div id="mobile-nav" class="hidden lg:hidden border-t border-stone-200 bg-white px-4 py-3 space-y-2">
      <div class="flex items-center justify-between py-1">
        <span class="text-xs font-semibold text-stone-600">Your Table:</span>
        <select id="mobile-table-select" onchange="changeTable(this.value)" class="bg-stone-50 border border-stone-300 text-terracotta-700 text-xs font-bold rounded-xl px-2.5 py-1.5">
        </select>
      </div>
      <div class="grid grid-cols-5 gap-1 pt-1 text-center text-xs font-bold">
        <button onclick="switchTab('menu'); toggleMobileNav()" class="py-2 rounded-xl bg-stone-100 text-stone-800">Menu</button>
        <button onclick="switchTab('favs'); toggleMobileNav()" class="py-2 rounded-xl bg-stone-100 text-stone-800">Saved</button>
        <button onclick="switchTab('orders'); toggleMobileNav()" class="py-2 rounded-xl bg-stone-100 text-stone-800">Orders</button>
        <button onclick="switchTab('kds'); toggleMobileNav()" class="py-2 rounded-xl bg-stone-100 text-stone-800">KDS</button>
        <button onclick="openCyberGame(); toggleMobileNav()" class="py-2 rounded-xl bg-cyan-100 text-cyan-900 font-extrabold">⚡ Play</button>
      </div>
    </div>
  </header>

  <!-- Hero Header Summary -->
  <section class="border-b border-stone-200/80 bg-white py-6 px-4 sm:px-6">
    <div class="max-w-7xl mx-auto flex flex-col md:flex-row md:items-center justify-between gap-4">
      <div>
        <div class="flex items-center gap-2">
          <span class="px-2.5 py-0.5 rounded-full bg-sage-100 text-sage-700 border border-sage-600/20 text-[11px] font-bold uppercase tracking-wider">
            ● Seated Kitchen Open
          </span>
          <span class="text-xs text-stone-500 font-medium">TableBite Restaurant</span>
        </div>
        <h1 class="text-2xl sm:text-3xl font-display font-extrabold text-stone-900 mt-1">
          Pick your food, sent straight to your table.
        </h1>
        <p class="text-xs sm:text-sm text-stone-500 max-w-xl mt-0.5">
          Made fresh per order. Pocket-friendly pricing from ₹29 to ₹179 with direct table service.
        </p>
      </div>

      <div class="flex items-center gap-3 text-xs bg-[#f4efe6] border border-stone-300/80 p-2.5 rounded-2xl">
        <div class="px-3 border-r border-stone-300 text-center">
          <div class="font-display font-bold text-base text-stone-900">20</div>
          <div class="text-[10px] text-stone-500 uppercase font-semibold">Tables</div>
        </div>
        <div class="px-3 border-r border-stone-300 text-center">
          <div class="font-display font-bold text-base text-terracotta-600">₹29</div>
          <div class="text-[10px] text-stone-500 uppercase font-semibold">Starts At</div>
        </div>
        <div class="px-3 text-center">
          <div class="font-display font-bold text-base text-emerald-700">5%</div>
          <div class="text-[10px] text-stone-500 uppercase font-semibold">Flat GST</div>
        </div>
      </div>
    </div>
  </section>

  <!-- Dynamic Views Container -->
  <main class="max-w-7xl mx-auto px-4 sm:px-6 py-6 flex-1 w-full">

    <!-- 1. Menu Tab View -->
    <div id="view-menu" class="space-y-6">
      <div class="bg-white border border-stone-200 rounded-3xl p-4 sm:p-5 shadow-xs space-y-4">
        <div class="flex flex-col sm:flex-row items-stretch sm:items-center justify-between gap-3">
          
          <!-- Search Field -->
          <div class="relative flex-1">
            <span class="absolute left-3.5 top-3 text-stone-400 text-sm">🔍</span>
            <input 
              type="text" 
              id="dish-search-input" 
              placeholder="Search dishes (samosa, biryani, paneer, lassi)..." 
              class="w-full bg-stone-50 border border-stone-200 rounded-2xl pl-9 pr-8 py-2.5 text-xs text-stone-800 placeholder-stone-400 focus:outline-none focus:ring-2 focus:ring-terracotta-500 focus:bg-white transition"
            />
            <button onclick="clearSearch()" id="clear-search-btn" class="hidden absolute right-3 top-2.5 text-stone-400 hover:text-stone-700 text-xs">✕</button>
          </div>

          <!-- Diet Toggle -->
          <div class="flex items-center bg-stone-100 p-1 rounded-2xl border border-stone-200 text-xs font-semibold">
            <button onclick="filterDiet('all')" id="diet-pill-all" class="px-3 py-1.5 rounded-xl bg-white text-stone-900 shadow-xs transition">All</button>
            <button onclick="filterDiet('veg')" id="diet-pill-veg" class="px-3 py-1.5 rounded-xl text-stone-600 hover:text-emerald-700 flex items-center gap-1.5 transition">
              <span class="w-2 h-2 rounded-full bg-emerald-600"></span> Veg
            </button>
            <button onclick="filterDiet('nonveg')" id="diet-pill-nonveg" class="px-3 py-1.5 rounded-xl text-stone-600 hover:text-rose-700 flex items-center gap-1.5 transition">
              <span class="w-2 h-2 rounded-full bg-rose-600"></span> Non-Veg
            </button>
          </div>

          <!-- Sort Select -->
          <select id="dish-sorter" class="bg-stone-50 border border-stone-200 text-xs font-bold text-stone-700 rounded-2xl px-3 py-2.5 focus:outline-none focus:ring-2 focus:ring-terracotta-500">
            <option value="recommended">Featured Items</option>
            <option value="price-asc">Price: Low to High</option>
            <option value="price-desc">Price: High to Low</option>
            <option value="rating">Top Rated</option>
          </select>
        </div>

        <!-- Category Filter Pills -->
        <div class="flex items-center gap-2 overflow-x-auto pb-1 no-scrollbar" id="category-scroller">
        </div>
      </div>

      <!-- Food Dish Cards Grid -->
      <div id="dish-gallery" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-5">
      </div>

      <!-- Empty Results State -->
      <div id="dish-gallery-empty" class="hidden text-center py-16 bg-white border border-dashed border-stone-300 rounded-3xl">
        <div class="text-3xl mb-2">🍽️</div>
        <h4 class="text-base font-bold text-stone-800">No dishes match your selection</h4>
        <p class="text-xs text-stone-500 mt-1 max-w-sm mx-auto">Try resetting your search query or picking a different category pill.</p>
        <button onclick="resetMenuFilters()" class="mt-4 px-4 py-2 bg-stone-900 text-white text-xs font-bold rounded-xl shadow-xs transition hover:bg-black">
          Reset All Filters
        </button>
      </div>
    </div>

    <!-- 2. Saved Favorites View -->
    <div id="view-favs" class="hidden space-y-4">
      <div class="flex items-center justify-between pb-3 border-b border-stone-300">
        <div>
          <h2 class="text-xl font-display font-bold text-stone-900">Your Saved Picks</h2>
          <p class="text-xs text-stone-500">Handpicked dishes for quick re-ordering at TableBite.</p>
        </div>
        <button onclick="switchTab('menu')" class="text-xs font-bold text-terracotta-600 hover:underline">
          ← Back to Menu
        </button>
      </div>

      <div id="favs-gallery" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-5"></div>

      <div id="favs-empty" class="hidden text-center py-16 bg-white border border-dashed border-stone-300 rounded-3xl">
        <div class="text-3xl mb-2">🤍</div>
        <h4 class="text-base font-bold text-stone-800">No favorites saved yet</h4>
        <p class="text-xs text-stone-500 mt-1">Tap the heart on any food card to bookmark it here.</p>
        <button onclick="switchTab('menu')" class="mt-4 px-4 py-2 bg-terracotta-600 text-white text-xs font-bold rounded-xl">
          Browse Menu
        </button>
      </div>
    </div>

    <!-- 3. Table Orders View -->
    <div id="view-orders" class="hidden space-y-4">
      <div class="flex items-center justify-between pb-3 border-b border-stone-300">
        <div>
          <h2 class="text-xl font-display font-bold text-stone-900">Active Table Orders</h2>
          <p class="text-xs text-stone-500">Real-time status updates directly from the TableBite pass.</p>
        </div>
        <div class="flex items-center gap-3">
          <button onclick="openCyberGame()" class="px-3 py-1.5 bg-cyan-100 hover:bg-cyan-200 border border-cyan-300 rounded-xl text-xs font-bold text-cyan-900 flex items-center gap-1 transition">
            <span>⚡</span> Play Cyber Line
          </button>
          <button onclick="switchTab('menu')" class="text-xs font-bold text-terracotta-600 hover:underline">
            + Add More Items
          </button>
        </div>

      <div id="table-orders-container" class="space-y-3"></div>

      <div id="table-orders-empty" class="hidden text-center py-16 bg-white border border-dashed border-stone-300 rounded-3xl">
        <div class="text-3xl mb-2">🧾</div>
        <h4 class="text-base font-bold text-stone-800">No active orders placed</h4>
        <p class="text-xs text-stone-500 mt-1">Select items from the menu and confirm your table number to start dining.</p>
        <button onclick="switchTab('menu')" class="mt-4 px-4 py-2 bg-terracotta-600 text-white text-xs font-bold rounded-xl">
          Browse Dishes
        </button>
      </div>
    </div>

    <!-- 4. Kitchen Display System (KDS) View -->
    <div id="view-kds" class="hidden space-y-4">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 pb-3 border-b border-stone-300">
        <div>
          <h2 class="text-xl font-display font-bold text-stone-900 flex items-center gap-2">
            TableBite Staff KDS <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-pulse"></span>
          </h2>
          <p class="text-xs text-stone-500">Ticket management for line cooks and kitchen staff.</p>
        </div>
        <div class="text-xs font-semibold text-emerald-800 bg-emerald-100 border border-emerald-300 px-2.5 py-1 rounded-xl">
          Live Order Feed
        </div>
      </div>

      <div id="kds-tickets-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4"></div>

      <div id="kds-empty" class="hidden text-center py-16 bg-white border border-dashed border-stone-300 rounded-3xl">
        <div class="text-3xl mb-2">🍳</div>
        <h4 class="text-base font-bold text-stone-800">Kitchen queue is clear</h4>
        <p class="text-xs text-stone-500 mt-1">All table tickets have been prepared and served.</p>
      </div>
    </div>

  </main>

  <!-- Slideout Cart Drawer -->
  <div id="cart-scrim" class="fixed inset-0 bg-stone-900/40 backdrop-blur-xs z-50 hidden transition-opacity" onclick="toggleCart(false)"></div>
  <aside id="cart-panel" class="fixed top-0 right-0 bottom-0 w-full sm:w-[420px] bg-white border-l border-stone-200 z-50 transform translate-x-full transition-transform duration-300 flex flex-col shadow-2xl">
    
    <div class="p-5 border-b border-stone-200 flex items-center justify-between bg-stone-50">
      <div>
        <h3 class="font-display font-bold text-stone-900 text-base">Your Table Cart</h3>
        <p id="drawer-subtitle" class="text-[11px] text-stone-500">0 items queued</p>
      </div>
      <button onclick="toggleCart(false)" class="w-8 h-8 rounded-xl bg-stone-200 hover:bg-stone-300 text-stone-700 flex items-center justify-center text-sm font-bold">
        ✕
      </button>
    </div>

    <div class="px-5 py-2.5 bg-amber-50 border-b border-amber-200/60 flex items-center justify-between text-xs">
      <span class="text-amber-800 font-medium">Ordering For:</span>
      <span id="drawer-table-indicator" class="font-bold text-amber-900 font-mono">Table 1</span>
    </div>

    <div id="drawer-items-list" class="flex-1 overflow-y-auto p-5 space-y-3 divide-y divide-stone-100"></div>

    <div id="drawer-empty" class="hidden flex-1 flex flex-col items-center justify-center p-6 text-center">
      <div class="text-4xl mb-2">🛒</div>
      <h4 class="text-sm font-bold text-stone-800">Your cart is empty</h4>
      <p class="text-xs text-stone-400 mt-1 max-w-xs">Pick your items from the menu to build your table ticket.</p>
      <button onclick="toggleCart(false); switchTab('menu')" class="mt-4 px-4 py-2 bg-stone-900 hover:bg-black text-white rounded-xl text-xs font-bold transition">
        Browse Dishes
      </button>
    </div>

    <div id="drawer-footer" class="p-5 border-t border-stone-200 bg-stone-50 space-y-3">
      <div class="space-y-1.5 text-xs text-stone-600">
        <div class="flex justify-between"><span>Dishes Subtotal</span><span id="calc-subtotal" class="font-mono font-bold text-stone-900">₹0</span></div>
        <div class="flex justify-between"><span>GST (5%)</span><span id="calc-tax" class="font-mono text-stone-700">₹0</span></div>
        <div class="flex justify-between text-emerald-700 font-semibold"><span>Dine-In Service Fee</span><span>₹0 (Free)</span></div>
        <div class="pt-2 border-t border-stone-300 flex justify-between text-sm font-extrabold text-stone-900">
          <span>Total Payable</span>
          <span id="calc-grandtotal" class="font-mono text-terracotta-600 text-base">₹0</span>
        </div>
      </div>

      <button onclick="openCheckoutModal()" class="w-full py-3.5 bg-terracotta-600 hover:bg-terracotta-700 text-white rounded-xl font-bold text-xs uppercase tracking-wider shadow-sm transition">
        Confirm & Send To Kitchen →
      </button>
    </div>
  </aside>

  <!-- Checkout Modal Dialog -->
  <div id="order-modal" class="fixed inset-0 z-50 bg-stone-900/60 backdrop-blur-xs hidden items-center justify-center p-4">
    <div class="bg-white border border-stone-200 rounded-3xl max-w-md w-full p-6 shadow-2xl space-y-4">
      <div class="flex items-center justify-between pb-3 border-b border-stone-100">
        <div>
          <h3 class="font-display font-bold text-stone-900 text-base">Confirm Order Ticket</h3>
          <p class="text-xs text-stone-500">Order will be sent straight to the chef pass</p>
        </div>
        <button onclick="closeCheckoutModal()" class="text-stone-400 hover:text-stone-700 text-sm font-bold">✕</button>
      </div>

      <form id="checkout-form" class="space-y-3.5 text-xs">
        <div>
          <label for="modal-table-select" class="block font-bold text-stone-700 mb-1">Confirm Table Number *</label>
          <select id="modal-table-select" required class="w-full bg-stone-50 border border-stone-300 text-stone-900 font-bold rounded-xl p-2.5 focus:outline-none focus:ring-2 focus:ring-terracotta-500">
          </select>
        </div>

        <div class="grid grid-cols-2 gap-2">
          <div>
            <label for="modal-cust-name" class="block font-bold text-stone-700 mb-1">Your Name *</label>
            <input type="text" id="modal-cust-name" required placeholder="Guest name" class="w-full bg-stone-50 border border-stone-300 rounded-xl p-2.5 text-stone-900 focus:outline-none focus:ring-2 focus:ring-terracotta-500" />
          </div>
          <div>
            <label for="modal-cust-phone" class="block font-bold text-stone-700 mb-1">Phone Number *</label>
            <input type="tel" id="modal-cust-phone" required pattern="[0-9]{10}" placeholder="10 digits" class="w-full bg-stone-50 border border-stone-300 rounded-xl p-2.5 text-stone-900 focus:outline-none focus:ring-2 focus:ring-terracotta-500" />
          </div>
        </div>

        <div>
          <label for="modal-cust-notes" class="block font-bold text-stone-700 mb-1">Kitchen Instructions</label>
          <input type="text" id="modal-cust-notes" placeholder="e.g. Less spicy, warm water..." class="w-full bg-stone-50 border border-stone-300 rounded-xl p-2.5 text-stone-900 focus:outline-none focus:ring-2 focus:ring-terracotta-500" />
        </div>

        <div>
          <label class="block font-bold text-stone-700 mb-1">Payment Method</label>
          <div class="grid grid-cols-3 gap-2">
            <label class="pay-method-pill border-2 border-terracotta-500 bg-terracotta-50 rounded-xl p-2.5 text-center cursor-pointer block">
              <input type="radio" name="paymethod" value="Table UPI / QR" checked class="hidden" onchange="selectPaymentMethod(this)">
              <div class="font-bold text-stone-900">UPI QR</div>
              <div class="text-[10px] text-terracotta-700">At Table</div>
            </label>
            <label class="pay-method-pill border border-stone-200 bg-white rounded-xl p-2.5 text-center cursor-pointer block hover:border-stone-400">
              <input type="radio" name="paymethod" value="Card Machine" class="hidden" onchange="selectPaymentMethod(this)">
              <div class="font-bold text-stone-900">Card</div>
              <div class="text-[10px] text-stone-500">POS Machine</div>
            </label>
            <label class="pay-method-pill border border-stone-200 bg-white rounded-xl p-2.5 text-center cursor-pointer block hover:border-stone-400">
              <input type="radio" name="paymethod" value="Cash to Server" class="hidden" onchange="selectPaymentMethod(this)">
              <div class="font-bold text-stone-900">Cash</div>
              <div class="text-[10px] text-stone-500">To Waiter</div>
            </label>
          </div>
        </div>

        <div class="p-3 bg-stone-100 rounded-xl flex items-center justify-between font-medium">
          <span class="text-stone-600">Total Billed Amount:</span>
          <span id="modal-final-amount" class="text-base font-mono font-black text-terracotta-600">₹0</span>
        </div>

        <button type="submit" class="w-full py-3.5 bg-stone-900 hover:bg-black text-white font-bold text-xs uppercase tracking-wider rounded-xl transition">
          Confirm Order
        </button>
      </form>
    </div>
  </div>

  <!-- CYBER LINE: 90° TURN GAME MODAL -->
  <div id="cyber-modal" class="fixed inset-0 z-50 bg-stone-950/90 backdrop-blur-md hidden items-center justify-center p-3 sm:p-4 select-none">
    <div id="cyber-card" class="bg-[#0b0f19] border border-cyan-900/50 text-white rounded-3xl max-w-md w-full p-4 sm:p-5 shadow-2xl relative flex flex-col justify-between transition-all duration-300">
      
      <!-- Top Status Header -->
      <div class="flex items-center justify-between pb-3 border-b border-cyan-950">
        <div class="flex items-center gap-2">
          <span class="text-xl">⚡</span>
          <div>
            <h3 class="font-display font-black text-sm tracking-wide text-cyan-400 uppercase">Cyber Line: 90° Turn</h3>
            <p class="text-[10px] text-slate-400">Tap screen or Space to switch direction!</p>
          </div>
        </div>
        <div class="flex items-center gap-2">
          <span class="px-2.5 py-1 rounded-xl bg-cyan-950/80 font-mono text-xs font-bold text-cyan-300 border border-cyan-800/60" id="cyber-score-pill">Score: 0</span>
          <button onclick="closeCyberGame()" class="px-2.5 py-1 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs font-semibold transition">
            ✕
          </button>
        </div>
      </div>

      <!-- Quick Objective & Gems Status -->
      <div class="py-2.5 flex items-center justify-between text-xs text-slate-400 font-mono">
        <div>Best: <strong id="cyber-best-score" class="text-amber-400">0</strong></div>
        <div>Crystals: <strong id="cyber-gems-display" class="text-cyan-300">💎 0</strong></div>
      </div>

      <!-- Canvas Play Area -->
      <div class="relative w-full aspect-[4/5] bg-gradient-to-b from-[#060814] to-[#0d1527] rounded-2xl border border-cyan-900/40 overflow-hidden flex items-center justify-center shadow-inner cursor-pointer" onclick="handleCyberTap()">
        <canvas id="cyber-canvas" width="380" height="475" class="w-full h-full block"></canvas>

        <!-- Tap to Start Prompt -->
        <div id="cyber-start-overlay" class="absolute inset-0 bg-black/40 backdrop-blur-2xs flex flex-col items-center justify-center p-6 text-center space-y-3 z-10 pointer-events-none">
          <div class="w-12 h-12 rounded-2xl bg-cyan-500/20 border border-cyan-400/40 flex items-center justify-center text-cyan-300 text-2xl animate-pulse">
            ⚡
          </div>
          <h4 class="text-lg font-display font-black text-white tracking-wide">TAP TO RUN</h4>
          <p class="text-xs text-cyan-200/80 max-w-xs leading-relaxed">
            Every tap switches your direction by 90°. Don't fall into the void!
          </p>
          <span class="text-[11px] font-mono text-cyan-400 bg-cyan-950/80 px-3 py-1 rounded-full border border-cyan-800/60">
            Click / Tap / Spacebar
          </span>
        </div>

        <!-- Game Over Screen -->
        <div id="cyber-gameover-overlay" class="absolute inset-0 bg-[#060814]/90 backdrop-blur-xs hidden flex-col items-center justify-center p-6 text-center space-y-3 z-20 animate-fade-in pointer-events-auto">
          <div class="text-3xl">💥</div>
          <h4 class="text-lg font-display font-extrabold text-rose-400 tracking-wide">OFF THE GRID!</h4>
          
          <div class="bg-cyan-950/60 border border-cyan-900/60 rounded-2xl py-3 px-6 text-xs space-y-1 font-mono text-slate-300">
            <div>SCORE: <strong id="gameover-score-val" class="text-cyan-300 text-sm">0</strong></div>
            <div>CRYSTALS: <strong id="gameover-gems-val" class="text-amber-400 text-sm">0</strong></div>
            <div>RECORD: <strong id="gameover-best-val" class="text-white text-sm">0</strong></div>
          </div>

          <div class="flex gap-2 pt-2">
            <button onclick="event.stopPropagation(); startCyberGame();" class="px-5 py-2.5 bg-cyan-500 hover:bg-cyan-400 text-slate-950 font-display font-extrabold rounded-xl text-xs uppercase tracking-wider transition shadow-lg shadow-cyan-500/30">
              ⚡ Play Again
            </button>
          </div>
        </div>
      </div>

      <!-- Bottom Bar -->
      <div class="pt-3 mt-2 border-t border-cyan-950 flex items-center justify-between text-xs">
        <span class="text-slate-500 font-mono text-[11px]">Geometric 90° Reflex</span>
        <button onclick="closeCyberGame()" class="text-amber-400 hover:text-amber-300 font-bold hover:underline">
          My Food Is Here! (Exit Game) →
        </button>
      </div>

    </div>
  </div>

  <!-- Notification Toast -->
  <div id="bite-toast" class="fixed bottom-5 right-5 z-50 bg-stone-900 text-white text-xs px-4 py-3 rounded-2xl shadow-xl flex items-center gap-2.5 hidden"></div>

  <!-- Footer -->
  <footer class="border-t border-stone-300 bg-white py-8 text-xs text-stone-500 mt-auto">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 flex flex-col md:flex-row items-center justify-between gap-4 text-center md:text-left">
      <div>
        <span class="font-display font-bold text-stone-800 text-sm">TableBite Restaurant</span>
        <p class="text-[11px] text-stone-500 mt-0.5">Dine-in self ordering portal project.</p>
      </div>
      <div class="flex items-center gap-6">
        <span>Dining Hall: Tables 1 to 20</span>
        <span>•</span>
        <span>Kitchen Timings: 11:00 AM to 11:00 PM</span>
      </div>
    </div>
  </footer>

  <script>
    // Food items catalogue with reliable direct image links
    // Food items catalogue
 const menuItems = [
      {
        id: "tb-samosa",
        name: "Crispy Punjabi Samosa (2 pcs)",
        category: "Small Bites",
        price: 39,
        rating: 4.8,
        diet: "veg",
        image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTujp-F2QedXl6ZWBvhrX1FtiU7lLe8bvuoi_YxU0FXhw&s=10",
        description: "Golden flaky triangular pastry stuffed with spiced potatoes, ginger and peas."
      },
      {
        id: "tb-paneertikka",
        name: "Charred Paneer Tikka Skewers",
        category: "Small Bites",
        price: 129,
        rating: 4.8,
        diet: "veg",
        image: "https://www.cookwithmanali.com/wp-content/uploads/2015/07/Restaurant-Style-Recipe-Paneer-Tikka.jpg",
        description: "Tandoor roasted cottage cheese cubes skewered with capsicum and charred onion."
      },
      {
        id: "tb-chickentikka",
        name: "Tandoori Chicken Tikka",
        category: "Small Bites",
        price: 149,
        rating: 4.9,
        diet: "nonveg",
        image: "https://shahzadidevje.com/wp-content/uploads/2023/02/Tandoori-Chicken-tikka-2-2.jpg",
        description: "Boneless chicken chunks rubbed in mustard oil, yogurt, red chili, and charcoal-grilled."
      },

      {
        id: "tb-paneerbutter",
        name: "Paneer Butter Masala",
        category: "Main Curries",
        price: 139,
        rating: 4.8,
        diet: "veg",
        image: "https://images.unsplash.com/photo-1631452180519-c014fe946bc7?auto=format&fit=crop&w=650&q=80",
        description: "Soft paneer cubes submerged in rich tomato, butter, and cashew creamy makhani gravy."
      },
      {
        id: "tb-butternon",
        name: "butter non with paneer butter masala",
        category: "Main Curries",
        price: 119,
        rating: 4.8,
        diet: "veg",
        image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQXjFktK5-du22DdznSHqDxEHjrYcGw7E9ByhGuCDpR_w&s=10",
        description: "Whole black urad lentils slow-simmered overnight with white butter and fresh cream."
      },
      {
        id: "tb-butterchicken",
        name: "Delhi Butter Chicken",
        category: "Main Curries",
        price: 169,
        rating: 4.9,
        diet: "nonveg",
        image: "https://images.unsplash.com/photo-1603894584373-5ac82b2ae398?auto=format&fit=crop&w=650&q=80",
        description: "Smoky boneless tandoori chicken simmered in spiced buttery tomato gravy."
      },
      {
        id: "tb-biryani",
        name: "Hyderabadi Dum Biryani",
        category: "Main Curries",
        price: 179,
        rating: 4.9,
        diet: "nonveg",
        image: "https://theyummydelights.com/wp-content/uploads/2018/06/hyderabadi-chicken-dum-biryani.jpg",
        description: "Layered basmati rice and marinated chicken cooked on dum with mint and saffron."
      },
      {
        id: "tb-burger",
        name: "crispy chicken burger",
        category: "Breads & chicken",
        price: 99,
        rating: 4.8,
        diet: "nonveg",
        image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT-_mN_aQx5woFyOErgoiWOI6loskSTh7VAQFIDm0IwCQ&s=10",
        description: "Charcoal baked flatbread brushed with garlic butter and fresh cilantro."
      },
      {
        id: "tb-rice",
        name: "Steamed Aged Basmati Rice",
        category: "Breads & Rice",
        price: 49,
        rating: 4.6,
        diet: "veg",
        image: "https://images.unsplash.com/photo-1516684732162-798a0062be99?auto=format&fit=crop&w=650&q=80",
        description: "Fluffy, fragrant long-grain steamed rice cooked to tender perfection."
      },
      {
        id: "tb-gulabjamun",
        name: "Warm Gulab Jamun (2 pcs)",
        category: "Sweet Treats",
        price: 49,
        rating: 4.9,
        diet: "veg",
        image: "https://cdn1.foodviva.com/static-content/food-images/indian-recipes/gulab-jamun-gits-ready-mix/gulab-jamun-gits-ready-mix.jpg",
        description: "Traditional milk-solid dumplings soaked in warm rose water and cardamom sugar syrup."
      },
      {
        id: "tb-brownie",
        name: "Warm Fudgy Choco Brownie",
        category: "Sweet Treats",
        price: 79,
        rating: 4.8,
        diet: "veg",
        image: "https://images.unsplash.com/photo-1606313564200-e75d5e30476c?auto=format&fit=crop&w=650&q=80",
        description: "Rich dark chocolate baked brownie square served warm with chocolate drizzle."
      },
      {
        id: "tb-limesoda",
        name: "Fresh Mint Lime Soda",
        category: "Beverages",
        price: 39,
        rating: 4.7,
        diet: "veg",
        image: "https://images.unsplash.com/photo-1513558161293-cdaf765ed2fd?auto=format&fit=crop&w=650&q=80",
        description: "Sparkling club soda with fresh lime juice, crushed mint sprigs, and rock salt."
      },
      {
        id: "tb-coldcoffee",
        name: "Iced Blended Cold Coffee",
        category: "Beverages",
        price: 69,
        rating: 4.8,
        diet: "veg",
        image: "https://images.unsplash.com/photo-1517701604599-bb29b565090c?auto=format&fit=crop&w=650&q=80",
        description: "Espresso roast coffee whipped with cold creamy milk and light chocolate dust."
      },
      {
        id: "tb-chai",
        name: "Clay Pot Masala Chai",
        category: "Beverages",
        price: 29,
        rating: 4.9,
        diet: "veg",
        image: "https://images.unsplash.com/photo-1576092768241-dec231879fc3?auto=format&fit=crop&w=650&q=80",
        description: "Assam tea brewed with crushed cardamom and ginger served hot in an earthen kulhad."
      }
     ];

     const categories = ["All", "Small Bites", "Main Curries", "Breads & Rice", "Sweet Treats", "Beverages"]; 

    // App state
    let cart = {}; 
    let favorites = new Set();
    let tableOrders = [];
    let activeCategory = "All";
    let activeDiet = "all";
    let searchFilter = "";
    let sortOrder = "recommended";
    let selectedTable = "Table 1";
    let toastTimer = null;

    // Table Select Initialization
    function initTables() {
      const headerSelect = document.getElementById("header-table-select");
      const mobileSelect = document.getElementById("mobile-table-select");
      const modalSelect = document.getElementById("modal-table-select");

      let optionsHtml = "";
      for (let i = 1; i <= 20; i++) {
        optionsHtml += `<option value="Table ${i}">Table ${i}</option>`;
      }

      if (headerSelect) headerSelect.innerHTML = optionsHtml;
      if (mobileSelect) mobileSelect.innerHTML = optionsHtml;
      if (modalSelect) modalSelect.innerHTML = optionsHtml;

      const saved = localStorage.getItem("tb_selected_table");
      if (saved) {
        selectedTable = saved;
        changeTable(saved);
      }
    }

    function changeTable(val) {
      selectedTable = val;
      localStorage.setItem("tb_selected_table", val);
      
      const headerSelect = document.getElementById("header-table-select");
      const mobileSelect = document.getElementById("mobile-table-select");
      const modalSelect = document.getElementById("modal-table-select");
      const indicator = document.getElementById("drawer-table-indicator");

      if (headerSelect) headerSelect.value = val;
      if (mobileSelect) mobileSelect.value = val;
      if (modalSelect) modalSelect.value = val;
      if (indicator) indicator.textContent = val;
    }

    // Local Storage synchronization
    function loadStorageData() {
      try {
        const savedCart = localStorage.getItem("tb_cart");
        if (savedCart) cart = JSON.parse(savedCart);

        const savedFavs = localStorage.getItem("tb_favs");
        if (savedFavs) favorites = new Set(JSON.parse(savedFavs));

        const savedOrders = localStorage.getItem("tb_orders");
        if (savedOrders) tableOrders = JSON.parse(savedOrders);
      } catch (e) {
        console.error("Storage load issue:", e);
      }
    }

    function saveCart() {
      localStorage.setItem("tb_cart", JSON.stringify(cart));
      updateCartUI();
      renderMenu();
      renderFavorites();
    }

    function saveFavorites() {
      localStorage.setItem("tb_favs", JSON.stringify([...favorites]));
      updateFavBadge();
      renderMenu();
      renderFavorites();
    }

    function saveOrders() {
      localStorage.setItem("tb_orders", JSON.stringify(tableOrders));
      renderOrders();
      renderKDS();
    }

    // Category Buttons
    function renderCategories() {
      const container = document.getElementById("category-scroller");
      if (!container) return;

      container.innerHTML = categories.map(cat => {
        const isActive = activeCategory === cat;
        return `
          <button 
            onclick="setCategory('${cat}')"
            class="px-4 py-2 rounded-2xl text-xs font-bold whitespace-nowrap transition ${
              isActive 
                ? 'bg-terracotta-600 text-white shadow-xs' 
                : 'bg-stone-100 text-stone-600 hover:bg-stone-200 border border-stone-200'
            }"
          >
            ${cat}
          </button>
        `;
      }).join("");
    }

    // Menu Cards rendering
    function renderMenu() {
      const gallery = document.getElementById("dish-gallery");
      const emptyAlert = document.getElementById("dish-gallery-empty");
      if (!gallery) return;

      const query = searchFilter.toLowerCase().trim();

      let filtered = menuItems.filter(item => {
        const matchesCategory = activeCategory === "All" || item.category === activeCategory;
        const matchesDiet = activeDiet === "all" || item.diet === activeDiet;
        const matchesSearch = !query || 
          item.name.toLowerCase().includes(query) || 
          item.description.toLowerCase().includes(query);

        return matchesCategory && matchesDiet && matchesSearch;
      });

      if (sortOrder === "price-asc") filtered.sort((a, b) => a.price - b.price);
      else if (sortOrder === "price-desc") filtered.sort((a, b) => b.price - a.price);
      else if (sortOrder === "rating") filtered.sort((a, b) => b.rating - a.rating);

      if (filtered.length === 0) {
        gallery.innerHTML = "";
        if (emptyAlert) emptyAlert.classList.remove("hidden");
        return;
      }

      if (emptyAlert) emptyAlert.classList.add("hidden");
      gallery.innerHTML = filtered.map(dish => buildDishCard(dish)).join("");
    }

    function buildDishCard(dish) {
      const qty = cart[dish.id] || 0;
      const isFav = favorites.has(dish.id);

      return `
        <div class="bg-white border border-stone-200/90 rounded-3xl overflow-hidden flex flex-col justify-between hover:shadow-lg transition-all duration-300 group">
          <div class="relative h-44 bg-stone-100 overflow-hidden">
            <img 
              src="${dish.image}" 
              alt="${dish.name}" 
              loading="lazy"
              referrerpolicy="no-referrer"
              onerror="this.onerror=null; this.src='https://images.unsplash.com/photo-1546069901-ba9599a7e63c?auto=format&fit=crop&w=700&q=80';"
              class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
            />
            
            <button 
              onclick="toggleFavorite('${dish.id}')"
              class="absolute top-3 right-3 w-8 h-8 rounded-full bg-white/90 backdrop-blur-md text-xs flex items-center justify-center border border-stone-200 shadow-sm hover:scale-110 transition"
              title="Save favorite"
            >
              ${isFav ? '❤️' : '🤍'}
            </button>

            <div class="absolute bottom-3 left-3 flex items-center gap-1.5">
              <span class="text-[10px] font-bold px-2 py-0.5 rounded-lg bg-white/95 backdrop-blur-md border border-stone-200 text-stone-800 flex items-center gap-1 shadow-xs">
                <span class="w-2 h-2 rounded-full ${dish.diet === 'veg' ? 'bg-emerald-600' : 'bg-rose-600'}"></span>
                ${dish.diet === 'veg' ? 'Veg' : 'Non-Veg'}
              </span>
              <span class="text-[10px] font-bold px-2 py-0.5 rounded-lg bg-stone-900/90 text-white backdrop-blur-md shadow-xs">
                ★ ${dish.rating}
              </span>
            </div>
          </div>

          <div class="p-4 flex-1 flex flex-col justify-between gap-3">
            <div>
              <span class="text-[10px] uppercase font-bold text-terracotta-600 tracking-wider">${dish.category}</span>
              <h4 class="font-display font-bold text-stone-900 text-sm leading-snug line-clamp-1 mt-0.5">${dish.name}</h4>
              <p class="text-xs text-stone-500 mt-1 line-clamp-2 leading-relaxed">${dish.description}</p>
            </div>

            <div class="flex items-center justify-between pt-2 border-t border-stone-100">
              <div class="font-mono font-extrabold text-base text-stone-900">₹${dish.price}</div>

              ${qty > 0 ? `
                <div class="flex items-center gap-2 bg-stone-900 text-white rounded-xl px-2 py-1 shadow-xs">
                  <button onclick="updateQty('${dish.id}', -1)" class="w-5 h-5 flex items-center justify-center text-stone-300 hover:text-white font-bold">－</button>
                  <span class="font-mono text-xs font-bold text-white w-4 text-center">${qty}</span>
                  <button onclick="updateQty('${dish.id}', 1)" class="w-5 h-5 flex items-center justify-center text-stone-300 hover:text-white font-bold">＋</button>
                </div>
              ` : `
                <button 
                  onclick="addToCart('${dish.id}')"
                  class="px-3.5 py-1.5 bg-stone-100 hover:bg-stone-900 hover:text-white text-stone-800 border border-stone-200 text-xs font-bold rounded-xl transition flex items-center gap-1 active:scale-95 shadow-2xs"
                >
                  <span>＋</span> Add
                </button>
              `}
            </div>
          </div>
        </div>
      `;
    }

    // Filter controls
    function setCategory(cat) {
      activeCategory = cat;
      renderCategories();
      renderMenu();
    }

    function filterDiet(diet) {
      activeDiet = diet;
      ['all', 'veg', 'nonveg'].forEach(d => {
        const btn = document.getElementById(`diet-pill-${d}`);
        if (!btn) return;
        if (d === diet) {
          btn.className = "px-3 py-1.5 rounded-xl bg-white text-stone-900 shadow-xs transition";
        } else {
          btn.className = "px-3 py-1.5 rounded-xl text-stone-600 hover:text-stone-900 transition";
        }
      });
      renderMenu();
    }

    function clearSearch() {
      const input = document.getElementById("dish-search-input");
      if (input) input.value = "";
      searchFilter = "";
      document.getElementById("clear-search-btn").classList.add("hidden");
      renderMenu();
    }

    function resetMenuFilters() {
      activeCategory = "All";
      activeDiet = "all";
      searchFilter = "";
      sortOrder = "recommended";
      
      const searchInput = document.getElementById("dish-search-input");
      const sorter = document.getElementById("dish-sorter");
      if (searchInput) searchInput.value = "";
      if (sorter) sorter.value = "recommended";

      filterDiet("all");
      renderCategories();
      renderMenu();
    }

    // Cart calculations
    function addToCart(id) {
      cart[id] = (cart[id] || 0) + 1;
      saveCart();
      showToast("Item added to table cart");
    }

    function updateQty(id, delta) {
      if (!cart[id]) return;
      const next = cart[id] + delta;
      if (next <= 0) {
        delete cart[id];
      } else {
        cart[id] = next;
      }
      saveCart();
    }

    function removeFromCart(id) {
      delete cart[id];
      saveCart();
    }

    function calculateTotals() {
      let subtotal = 0;
      let totalCount = 0;

      for (const [id, count] of Object.entries(cart)) {
        const item = menuItems.find(m => m.id === id);
        if (item) {
          subtotal += item.price * count;
          totalCount += count;
        }
      }

      const tax = Math.round(subtotal * 0.05); // 5% GST
      const grandTotal = subtotal + tax;
      return { subtotal, tax, grandTotal, totalCount };
    }

    function updateCartUI() {
      const { subtotal, tax, grandTotal, totalCount } = calculateTotals();

      const badge = document.getElementById("cart-item-badge");
      const pricePill = document.getElementById("cart-price-pill");
      const subtitle = document.getElementById("drawer-subtitle");
      const listContainer = document.getElementById("drawer-items-list");
      const emptyContainer = document.getElementById("drawer-empty");
      const footerContainer = document.getElementById("drawer-footer");

      if (badge) badge.textContent = totalCount;
      if (pricePill) pricePill.textContent = `₹${subtotal}`;
      if (subtitle) subtitle.textContent = `${totalCount} item${totalCount === 1 ? '' : 's'} queued`;

      if (totalCount === 0) {
        if (listContainer) listContainer.innerHTML = "";
        if (emptyContainer) emptyContainer.classList.remove("hidden");
        if (footerContainer) footerContainer.classList.add("hidden");
        return;
      }

      if (emptyContainer) emptyContainer.classList.add("hidden");
      if (footerContainer) footerContainer.classList.remove("hidden");

      if (listContainer) {
        listContainer.innerHTML = Object.entries(cart).map(([id, count]) => {
          const item = menuItems.find(m => m.id === id);
          if (!item) return "";
          return `
            <div class="pt-3 flex items-center justify-between gap-3 text-xs">
              <div class="flex-1 min-w-0">
                <h5 class="font-display font-bold text-stone-900 truncate">${item.name}</h5>
                <div class="text-stone-500 text-[11px] font-mono mt-0.5">
                  ₹${item.price} × ${count} = <span class="text-stone-900 font-bold">₹${item.price * count}</span>
                </div>
              </div>

              <div class="flex items-center gap-1.5 bg-stone-100 rounded-xl px-2 py-1">
                <button onclick="updateQty('${item.id}', -1)" class="font-bold text-stone-600 hover:text-stone-900 px-1">－</button>
                <span class="font-mono text-xs font-bold text-stone-800">${count}</span>
                <button onclick="updateQty('${item.id}', 1)" class="font-bold text-stone-600 hover:text-stone-900 px-1">＋</button>
              </div>

              <button onclick="removeFromCart('${item.id}')" class="text-stone-400 hover:text-rose-600 text-xs px-1">✕</button>
            </div>
          `;
        }).join("");
      }

      document.getElementById("calc-subtotal").textContent = `₹${subtotal}`;
      document.getElementById("calc-tax").textContent = `₹${tax}`;
      document.getElementById("calc-grandtotal").textContent = `₹${grandTotal}`;
      document.getElementById("modal-final-amount").textContent = `₹${grandTotal}`;
    }

    function toggleCart(open) {
      const scrim = document.getElementById("cart-scrim");
      const panel = document.getElementById("cart-panel");
      if (!scrim || !panel) return;

      if (open) {
        scrim.classList.remove("hidden");
        panel.classList.remove("translate-x-full");
      } else {
        scrim.classList.add("hidden");
        panel.classList.add("translate-x-full");
      }
    }

    function toggleMobileNav() {
      const mob = document.getElementById("mobile-nav");
      if (mob) mob.classList.toggle("hidden");
    }

    // Favorites
    function toggleFavorite(id) {
      if (favorites.has(id)) {
        favorites.delete(id);
        showToast("Removed from favorites");
      } else {
        favorites.add(id);
        showToast("Saved to favorites");
      }
      saveFavorites();
    }

    function updateFavBadge() {
      const badge = document.getElementById("fav-counter-badge");
      if (!badge) return;

      if (favorites.size > 0) {
        badge.textContent = favorites.size;
        badge.classList.remove("hidden");
      } else {
        badge.classList.add("hidden");
      }
    }

    function renderFavorites() {
      const gallery = document.getElementById("favs-gallery");
      const empty = document.getElementById("favs-empty");
      if (!gallery) return;

      const list = menuItems.filter(item => favorites.has(item.id));

      if (list.length === 0) {
        gallery.innerHTML = "";
        if (empty) empty.classList.remove("hidden");
        return;
      }

      if (empty) empty.classList.add("hidden");
      gallery.innerHTML = list.map(dish => buildDishCard(dish)).join("");
    }

    // Checkout Modal
    function openCheckoutModal() {
      const { totalCount } = calculateTotals();
      if (totalCount === 0) {
        showToast("Your cart is empty");
        return;
      }
      toggleCart(false);
      document.getElementById("modal-table-select").value = selectedTable;
      const modal = document.getElementById("order-modal");
      modal.classList.remove("hidden");
      modal.classList.add("flex");
    }

    function closeCheckoutModal() {
      const modal = document.getElementById("order-modal");
      modal.classList.add("hidden");
      modal.classList.remove("flex");
    }

    function selectPaymentMethod(radio) {
      document.querySelectorAll(".pay-method-pill").forEach(pill => {
        pill.className = "pay-method-pill border border-stone-200 bg-white rounded-xl p-2.5 text-center cursor-pointer block hover:border-stone-400";
      });
      const current = radio.closest("label");
      if (current) {
        current.className = "pay-method-pill border-2 border-terracotta-500 bg-terracotta-50 rounded-xl p-2.5 text-center cursor-pointer block";
      }
    }

    function handleOrderSubmit(e) {
      e.preventDefault();

      const table = document.getElementById("modal-table-select").value;
      const guestName = document.getElementById("modal-cust-name").value.trim();
      const guestPhone = document.getElementById("modal-cust-phone").value.trim();
      const notes = document.getElementById("modal-cust-notes").value.trim();
      const payMethodInput = document.querySelector('input[name="paymethod"]:checked');
      const payMode = payMethodInput ? payMethodInput.value : "Table UPI / QR";
      const { grandTotal } = calculateTotals();

      if (!guestName || guestPhone.length !== 10) {
        alert("Please enter a valid guest name and 10-digit phone number.");
        return;
      }

      const itemsOrdered = Object.entries(cart).map(([id, count]) => {
        const item = menuItems.find(m => m.id === id);
        return {
          id: id,
          name: item ? item.name : "Custom Dish",
          price: item ? item.price : 0,
          count: count
        };
      });

      const orderTicket = {
        id: "TB-" + Date.now().toString().slice(-4),
        table: table,
        guest: guestName,
        phone: guestPhone,
        notes: notes,
        payMode: payMode,
        items: itemsOrdered,
        total: grandTotal,
        timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
        status: "Received"
      };

      tableOrders.unshift(orderTicket);
      cart = {};
      saveCart();
      saveOrders();

      closeCheckoutModal();
      switchTab("orders");
      showToast(`Ticket #${orderTicket.id} placed for ${table}`);
      
      document.getElementById("checkout-form").reset();

      // Automatically open the Cyber Line waiting game after the order is placed.
      // The game will remain open until the order is marked as Served.
      setTimeout(() => {
        openCyberGame();
        setTimeout(() => {
          if (document.getElementById("cyber-modal")?.classList.contains("flex")) {
            startCyberGame();
          }
        }, 250);
      }, 700);
    }

    // Orders Views
    function renderOrders() {
      const container = document.getElementById("table-orders-container");
      const empty = document.getElementById("table-orders-empty");
      if (!container) return;

      if (tableOrders.length === 0) {
        container.innerHTML = "";
        if (empty) empty.classList.remove("hidden");
        return;
      }

      if (empty) empty.classList.add("hidden");

      container.innerHTML = tableOrders.map(order => {
        let badgeClass = "bg-amber-100 text-amber-800 border-amber-300";
        if (order.status === "Cooking") badgeClass = "bg-sky-100 text-sky-800 border-sky-300";
        if (order.status === "Served") badgeClass = "bg-emerald-100 text-emerald-800 border-emerald-300";

        return `
          <div class="bg-white border border-stone-200 rounded-3xl p-5 shadow-xs space-y-3">
            <div class="flex items-center justify-between pb-3 border-b border-stone-100 text-xs">
              <div class="flex items-center gap-2">
                <span class="font-display font-extrabold text-stone-900 text-sm">#${order.id}</span>
                <span class="bg-stone-100 border border-stone-200 px-2 py-0.5 rounded-lg font-mono font-bold text-stone-800">${order.table}</span>
                <span class="text-stone-400">• ${order.timestamp}</span>
              </div>
              <span class="px-2.5 py-0.5 rounded-full text-[11px] font-bold border ${badgeClass}">
                ${order.status}
              </span>
            </div>

            <div class="space-y-1.5 text-xs">
              ${order.items.map(it => `
                <div class="flex justify-between items-center text-stone-700">
                  <span>${it.name} <span class="text-stone-400 font-mono">× ${it.count}</span></span>
                  <span class="font-mono font-bold text-stone-900">₹${it.price * it.count}</span>
                </div>
              `).join("")}
            </div>

            ${order.notes ? `
              <div class="text-[11px] text-amber-900 bg-amber-50 border border-amber-200 p-2.5 rounded-xl">
                <strong>Note:</strong> ${order.notes}
              </div>
            ` : ''}

            <div class="pt-3 border-t border-stone-100 flex items-center justify-between text-xs">
              <span class="text-stone-500">Method: ${order.payMode}</span>
              <span class="text-stone-700">Total Billed: <strong class="font-mono text-terracotta-600 text-sm">₹${order.total}</strong></span>
            </div>
          </div>
        `;
      }).join("");
    }

    // Kitchen Display System (KDS)
    function renderKDS() {
      const container = document.getElementById("kds-tickets-container");
      const empty = document.getElementById("kds-empty");
      if (!container) return;

      const pending = tableOrders.filter(o => o.status !== "Served");

      if (pending.length === 0) {
        container.innerHTML = "";
        if (empty) empty.classList.remove("hidden");
        return;
      }

      if (empty) empty.classList.add("hidden");

      container.innerHTML = pending.map(ord => {
        return `
          <div class="bg-white border border-stone-200 rounded-3xl p-5 shadow-xs space-y-3 flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between pb-2 border-b border-stone-100">
                <div>
                  <span class="font-display font-black text-terracotta-600 text-base">${ord.table}</span>
                  <span class="text-stone-400 text-xs ml-1">#${ord.id}</span>
                </div>
                <span class="font-mono text-xs text-stone-400">${ord.timestamp}</span>
              </div>

              <div class="py-2 text-[11px] text-stone-500">
                <div>Guest: <strong class="text-stone-800">${ord.guest}</strong> (${ord.phone})</div>
                ${ord.notes ? `<div class="mt-1 p-2 bg-amber-50 border border-amber-200 text-amber-800 rounded-xl">⚠️ ${ord.notes}</div>` : ''}
              </div>

              <div class="space-y-1.5 my-2">
                ${ord.items.map(it => `
                  <div class="flex justify-between items-center text-xs bg-stone-50 p-2 rounded-xl border border-stone-100">
                    <span class="font-semibold text-stone-800">${it.name}</span>
                    <span class="font-mono font-bold text-stone-900 bg-white px-2 py-0.5 rounded-lg border border-stone-200">×${it.count}</span>
                  </div>
                `).join("")}
              </div>
            </div>

            <div class="pt-3 border-t border-stone-100 grid grid-cols-2 gap-2 text-xs font-bold">
              <button 
                onclick="updateOrderStatus('${ord.id}', 'Cooking')" 
                class="py-2 rounded-xl border border-sky-300 text-sky-800 hover:bg-sky-50 transition ${ord.status === 'Cooking' ? 'bg-sky-50 ring-1 ring-sky-400' : ''}"
              >
                Cooking
              </button>
              <button 
                onclick="updateOrderStatus('${ord.id}', 'Served')" 
                class="py-2 rounded-xl bg-stone-900 hover:bg-black text-white transition"
              >
                Served ✓
              </button>
            </div>
          </div>
        `;
      }).join("");
    }

    function updateOrderStatus(id, newStatus) {
      const target = tableOrders.find(o => o.id === id);
      if (target) {
        target.status = newStatus;
        saveOrders();

        // The waiting game ends automatically when the food is served.
        if (newStatus === "Served") {
          closeCyberGame();
        }

        showToast(`Order #${target.id} updated to ${newStatus}`);
      }
    }

    // Tab Switching
    function switchTab(tab) {
      const tabs = ['menu', 'favs', 'orders', 'kds'];
      tabs.forEach(t => {
        const el = document.getElementById(`view-${t}`);
        const btn = document.getElementById(`btn-tab-${t}`);
        if (el) el.classList.add("hidden");
        if (btn) btn.className = "px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-stone-600 hover:text-stone-900 transition";
      });

      const activeView = document.getElementById(`view-${tab}`);
      const activeBtn = document.getElementById(`btn-tab-${tab}`);
      if (activeView) activeView.classList.remove("hidden");
      if (activeBtn) activeBtn.className = "px-4 py-2 rounded-xl text-xs font-bold uppercase tracking-wider text-white bg-terracotta-600 shadow-xs transition";

      if (tab === 'favs') renderFavorites();
      if (tab === 'orders') renderOrders();
      if (tab === 'kds') renderKDS();
    }

    // Toast Alert
    function showToast(message) {
      const toast = document.getElementById("bite-toast");
      if (!toast) return;

      toast.innerHTML = `<span>⚡</span> <span>${message}</span>`;
      toast.classList.remove("hidden");
      
      clearTimeout(toastTimer);
      toastTimer = setTimeout(() => {
        toast.classList.add("hidden");
      }, 2500);
    }

    /* -------------------------------------------------------------
     * "CYBER LINE: 90° TURN" ISOMETRIC RUNNER ENGINE
     * ------------------------------------------------------------- */
    let cyberCanvas, cyberCtx;
    let cyberAnimFrame = null;
    let cyberRunning = false;
    let cyberGameOver = false;

    const TILE_W = 42;
    const TILE_H = 21;
    const TILE_DEPTH = 18;

    let pathTiles = [];
    let crystals = [];
    let particles = [];

    let playerX = 0; // Grid space
    let playerY = 0;
    let playerZ = 0; // Falling physics
    let playerVz = 0;

    let dir = 0; // 0 = Moving along X (+X), 1 = Moving along Y (+Y)
    let speed = 0.055;
    let score = 0;
    let gemsCollected = 0;
    let bestScore = 0;

    function openCyberGame() {
      const modal = document.getElementById("cyber-modal");
      modal.classList.remove("hidden");
      modal.classList.add("flex");

      cyberCanvas = document.getElementById("cyber-canvas");
      cyberCtx = cyberCanvas.getContext("2d");

      bestScore = parseInt(localStorage.getItem("tb_cyber_best") || "0", 10);
      document.getElementById("cyber-best-score").textContent = bestScore;

      resetCyberWorld();
      drawCyberScene();
    }

    function closeCyberGame() {
      const modal = document.getElementById("cyber-modal");
      if (!modal) return;

      modal.classList.add("hidden");
      modal.classList.remove("flex");
      cyberRunning = false;
      cyberGameOver = false;
      if (cyberAnimFrame) {
        cancelAnimationFrame(cyberAnimFrame);
        cyberAnimFrame = null;
      }

      const startOverlay = document.getElementById("cyber-start-overlay");
      const gameOverOverlay = document.getElementById("cyber-gameover-overlay");
      if (startOverlay) startOverlay.classList.remove("hidden");
      if (gameOverOverlay) {
        gameOverOverlay.classList.add("hidden");
        gameOverOverlay.classList.remove("flex");
      }
    }

    function resetCyberWorld() {
      cyberRunning = false;
      cyberGameOver = false;
      score = 0;
      gemsCollected = 0;
      speed = 0.055;

      playerX = 0;
      playerY = 0;
      playerZ = 0;
      playerVz = 0;
      dir = 0;

      pathTiles = [];
      crystals = [];
      particles = [];

      // Generate initial straight platform
      for (let i = -2; i <= 3; i++) {
        pathTiles.push({ x: i, y: 0 });
      }

      // Generate procedural zig-zag forward
      let lastX = 3;
      let lastY = 0;
      let currentGenDir = 0;

      for (let i = 0; i < 45; i++) {
        const segLen = Math.floor(Math.random() * 3) + 2;
        currentGenDir = currentGenDir === 0 ? 1 : 0;

        for (let s = 0; s < segLen; s++) {
          if (currentGenDir === 0) lastX++;
          else lastY++;

          pathTiles.push({ x: lastX, y: lastY });

          // 20% chance to spawn a neon crystal
          if (Math.random() < 0.22) {
            crystals.push({ x: lastX, y: lastY, collected: false });
          }
        }
      }

      document.getElementById("cyber-score-pill").textContent = "Score: 0";
      document.getElementById("cyber-gems-display").textContent = "💎 0";
      document.getElementById("cyber-start-overlay").classList.remove("hidden");
      document.getElementById("cyber-gameover-overlay").classList.add("hidden");
      document.getElementById("cyber-gameover-overlay").classList.remove("flex");
    }

    function startCyberGame() {
      resetCyberWorld();
      cyberRunning = true;
      document.getElementById("cyber-start-overlay").classList.add("hidden");
      lastFrameTime = performance.now();
      requestAnimationFrame(cyberLoop);
    }

    function handleCyberTap() {
      if (cyberGameOver) return;

      if (!cyberRunning) {
        startCyberGame();
        return;
      }

      // 90° Turn
      dir = dir === 0 ? 1 : 0;
      score += 1;
      speed = Math.min(0.095, speed + 0.00035);

      document.getElementById("cyber-score-pill").textContent = `Score: ${score}`;

      // Spawn subtle turn trail particle
      const iso = gridToIso(playerX, playerY);
      createSparks(iso.x, iso.y, "#06b6d4", 3);
    }

    function gridToIso(gx, gy) {
      // Camera centers on player
      const cx = cyberCanvas.width / 2;
      const cy = cyberCanvas.height / 2 + 30;

      const relX = gx - playerX;
      const relY = gy - playerY;

      const isoX = cx + (relX - relY) * (TILE_W / 2);
      const isoY = cy + (relX + relY) * (TILE_H / 2);

      return { x: isoX, y: isoY };
    }

    let lastFrameTime = performance.now();

    function cyberLoop(timestamp) {
      if (!cyberRunning && !cyberGameOver) return;

      const dt = Math.min(2, (timestamp - lastFrameTime) / 16.66);
      lastFrameTime = timestamp;

      updateCyber(dt);
      drawCyberScene();

      cyberAnimFrame = requestAnimationFrame(cyberLoop);
    }

    function updateCyber(dt) {
      if (cyberRunning) {
        // Move player along current axis
        if (dir === 0) {
          playerX += speed * dt;
        } else {
          playerY += speed * dt;
        }

        // Check if player is on a tile
        const onTile = pathTiles.some(t => {
          return Math.abs(t.x - playerX) <= 0.65 && Math.abs(t.y - playerY) <= 0.65;
        });

        if (!onTile) {
          cyberRunning = false;
          cyberGameOver = true;
          playerVz = -0.05; // Fall into void
        }

        // Check Crystals
        crystals.forEach(c => {
          if (!c.collected && Math.hypot(c.x - playerX, c.y - playerY) < 0.5) {
            c.collected = true;
            gemsCollected++;
            score += 5;
            document.getElementById("cyber-gems-display").textContent = `💎 ${gemsCollected}`;
            document.getElementById("cyber-score-pill").textContent = `Score: ${score}`;
            const iso = gridToIso(c.x, c.y);
            createSparks(iso.x, iso.y - 12, "#38bdf8", 12);
          }
        });

        // Extend path continuously forward
        const lastTile = pathTiles[pathTiles.length - 1];
        if (lastTile.x - playerX < 25 || lastTile.y - playerY < 25) {
          let lx = lastTile.x;
          let ly = lastTile.y;
          const nextDir = Math.random() > 0.5 ? 0 : 1;
          const seg = Math.floor(Math.random() * 4) + 2;

          for (let s = 0; s < seg; s++) {
            if (nextDir === 0) lx++;
            else ly++;
            pathTiles.push({ x: lx, y: ly });
            if (Math.random() < 0.25) {
              crystals.push({ x: lx, y: ly, collected: false });
            }
          }
        }

        // Clean up old trailing tiles
        if (pathTiles.length > 70) {
          pathTiles = pathTiles.filter(t => (playerX - t.x < 10) && (playerY - t.y < 10));
          crystals = crystals.filter(c => (playerX - c.x < 10) && (playerY - c.y < 10));
        }
      } else if (cyberGameOver) {
        // Falling into void animation
        playerZ += playerVz * dt;
        playerVz -= 0.12 * dt;

        if (playerZ < -280) {
          cancelAnimationFrame(cyberAnimFrame);
          showCyberGameOver();
        }
      }

      // Update Particles
      for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i];
        p.x += p.vx * dt;
        p.y += p.vy * dt;
        p.alpha -= 0.03 * dt;
        if (p.alpha <= 0) particles.splice(i, 1);
      }
    }

    function createSparks(x, y, color, count) {
      for (let i = 0; i < count; i++) {
        const angle = Math.random() * Math.PI * 2;
        const spd = Math.random() * 3 + 1;
        particles.push({
          x: x,
          y: y,
          vx: Math.cos(angle) * spd,
          vy: Math.sin(angle) * spd,
          color: color,
          alpha: 1,
          size: Math.random() * 3 + 2
        });
      }
    }

    function showCyberGameOver() {
      if (score > bestScore) {
        bestScore = score;
        localStorage.setItem("tb_cyber_best", bestScore);
        document.getElementById("cyber-best-score").textContent = bestScore;
      }

      document.getElementById("gameover-score-val").textContent = score;
      document.getElementById("gameover-gems-val").textContent = gemsCollected;
      document.getElementById("gameover-best-val").textContent = bestScore;

      const screen = document.getElementById("cyber-gameover-overlay");
      screen.classList.remove("hidden");
      screen.classList.add("flex");
    }

    function drawCyberScene() {
      if (!cyberCtx) return;
      const w = cyberCanvas.width;
      const h = cyberCanvas.height;

      // Deep Space Void Background
      const bgGrad = cyberCtx.createRadialGradient(w / 2, h / 2, 40, w / 2, h / 2, w);
      bgGrad.addColorStop(0, "#0e1628");
      bgGrad.addColorStop(1, "#030712");
      cyberCtx.fillStyle = bgGrad;
      cyberCtx.fillRect(0, 0, w, h);

      // Distant Grid Lines
      cyberCtx.save();
      cyberCtx.strokeStyle = "rgba(6, 182, 212, 0.08)";
      cyberCtx.lineWidth = 1;
      for (let i = -w; i < w * 2; i += 36) {
        cyberCtx.beginPath();
        cyberCtx.moveTo(i, 0);
        cyberCtx.lineTo(i + h, h);
        cyberCtx.stroke();
      }
      cyberCtx.restore();

      // Draw Path Tiles in Isometric Order
      // Sort back-to-front
      const sortedTiles = [...pathTiles].sort((a, b) => (a.x + a.y) - (b.x + b.y));

      sortedTiles.forEach(tile => {
        const iso = gridToIso(tile.x, tile.y);
        drawIsoBlock(iso.x, iso.y, TILE_W, TILE_H, TILE_DEPTH);
      });

      // Draw Crystals
      crystals.forEach(c => {
        if (!c.collected) {
          const iso = gridToIso(c.x, c.y);
          drawNeonCrystal(iso.x, iso.y - 10);
        }
      });

      // Draw Glowing Player Orb
      const pIso = gridToIso(playerX, playerY);
      const ballY = pIso.y - 12 - playerZ;

      cyberCtx.save();
      // Neon Glow
      const glow = cyberCtx.createRadialGradient(pIso.x, ballY, 3, pIso.x, ballY, 18);
      glow.addColorStop(0, "rgba(255, 255, 255, 1)");
      glow.addColorStop(0.3, "rgba(56, 189, 248, 0.9)");
      glow.addColorStop(0.7, "rgba(6, 182, 212, 0.4)");
      glow.addColorStop(1, "rgba(6, 182, 212, 0)");
      cyberCtx.fillStyle = glow;
      cyberCtx.beginPath();
      cyberCtx.arc(pIso.x, ballY, 18, 0, Math.PI * 2);
      cyberCtx.fill();

      // Core Orb
      cyberCtx.beginPath();
      cyberCtx.arc(pIso.x, ballY, 6, 0, Math.PI * 2);
      cyberCtx.fillStyle = "#ffffff";
      cyberCtx.fill();
      cyberCtx.restore();

      // Draw Particles
      particles.forEach(p => {
        cyberCtx.save();
        cyberCtx.globalAlpha = p.alpha;
        cyberCtx.fillStyle = p.color;
        cyberCtx.beginPath();
        cyberCtx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        cyberCtx.fill();
        cyberCtx.restore();
      });
    }

    function drawIsoBlock(x, y, w, h, d) {
      const hw = w / 2;
      const hh = h / 2;

      // Top Face
      cyberCtx.beginPath();
      cyberCtx.moveTo(x, y - hh);
      cyberCtx.lineTo(x + hw, y);
      cyberCtx.lineTo(x, y + hh);
      cyberCtx.lineTo(x - hw, y);
      cyberCtx.closePath();
      cyberCtx.fillStyle = "#1e293b";
      cyberCtx.fill();
      cyberCtx.strokeStyle = "rgba(56, 189, 248, 0.4)";
      cyberCtx.lineWidth = 1;
      cyberCtx.stroke();

      // Left Face
      cyberCtx.beginPath();
      cyberCtx.moveTo(x - hw, y);
      cyberCtx.lineTo(x, y + hh);
      cyberCtx.lineTo(x, y + hh + d);
      cyberCtx.lineTo(x - hw, y + d);
      cyberCtx.closePath();
      cyberCtx.fillStyle = "#0f172a";
      cyberCtx.fill();

      // Right Face
      cyberCtx.beginPath();
      cyberCtx.moveTo(x + hw, y);
      cyberCtx.lineTo(x, y + hh);
      cyberCtx.lineTo(x, y + hh + d);
      cyberCtx.lineTo(x + hw, y + d);
      cyberCtx.closePath();
      cyberCtx.fillStyle = "#020617";
      cyberCtx.fill();
    }

    function drawNeonCrystal(x, y) {
      cyberCtx.save();
      const bob = Math.sin(performance.now() / 180) * 3;
      const cy = y + bob;

      cyberCtx.beginPath();
      cyberCtx.moveTo(x, cy - 8);
      cyberCtx.lineTo(x + 5, cy);
      cyberCtx.lineTo(x, cy + 8);
      cyberCtx.lineTo(x - 5, cy);
      cyberCtx.closePath();

      cyberCtx.fillStyle = "#38bdf8";
      cyberCtx.shadowColor = "#06b6d4";
      cyberCtx.shadowBlur = 10;
      cyberCtx.fill();
      cyberCtx.restore();
    }

    // Keyboard spacebar listener
    window.addEventListener("keydown", (e) => {
      const modal = document.getElementById("cyber-modal");
      if (modal.classList.contains("hidden")) return;

      if (e.code === "Space") {
        e.preventDefault();
        handleCyberTap();
      }
    });

    // DOM Ready listener
    document.addEventListener("DOMContentLoaded", () => {
      initTables();
      loadStorageData();
      renderCategories();
      renderMenu();
      updateCartUI();
      updateFavBadge();
      renderOrders();
      renderKDS();

      // Search input binding
      const searchInput = document.getElementById("dish-search-input");
      const clearBtn = document.getElementById("clear-search-btn");
      if (searchInput) {
        searchInput.addEventListener("input", (e) => {
          searchFilter = e.target.value;
          if (searchFilter.length > 0) {
            clearBtn.classList.remove("hidden");
          } else {
            clearBtn.classList.add("hidden");
          }
          renderMenu();
        });
      }

      // Sort binding
      const sorter = document.getElementById("dish-sorter");
      if (sorter) {
        sorter.addEventListener("change", (e) => {
          sortOrder = e.target.value;
          renderMenu();
        });
      }

      // Form submit binding
      const checkoutForm = document.getElementById("checkout-form");
      if (checkoutForm) {
        checkoutForm.addEventListener("submit", handleOrderSubmit);
      }
    });
  </script>
</body>
</html>
