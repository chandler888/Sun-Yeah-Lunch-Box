# Sun-Yeah-Lunch-Box
Website
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>上野烤肉飯 - 線上點餐</title>
  <script src="https://tailwindcss.com"></script>
  <style>
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans TC", sans-serif; }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
  </style>
</head>
<body class="bg-stone-100 text-stone-800 pb-28">

  <!-- 頂部店家資訊 -->
  <header class="bg-amber-900 text-white shadow-md sticky top-0 z-20">
    <div class="max-w-2xl mx-auto px-4 py-4">
      <div class="flex justify-between items-start">
        <div>
          <h1 class="text-xl font-bold tracking-wide">上野烤肉飯</h1>
          <p class="text-xs text-amber-200 mt-1">新竹縣湖口鄉中正路一段80號 ｜ ☎️ (03)599-3131</p>
        </div>
        <span class="bg-amber-700 text-amber-100 text-xs px-2.5 py-1 rounded-full font-medium shrink-0">星期日公休</span>
      </div>
      <div class="mt-2.5 bg-amber-950/60 text-amber-100 text-xs px-3 py-2 rounded-lg leading-relaxed">
        💡 中午外送請於 08:00 後電話預訂；電話預訂請依序排隊結帳領取便當。
      </div>
    </div>

    <!-- 分類導覽列 -->
    <nav id="category-tabs" class="bg-white text-stone-600 border-b border-stone-200 overflow-x-auto no-scrollbar">
      <div class="max-w-2xl mx-auto px-4 flex space-x-2 py-2.5 text-sm font-medium whitespace-nowrap">
        <button onclick="filterCategory('all')" class="tab-btn active px-3.5 py-1.5 rounded-full bg-amber-800 text-white" data-cat="all">全部餐點</button>
        <button onclick="filterCategory('烤類')" class="tab-btn px-3.5 py-1.5 rounded-full bg-stone-100 hover:bg-stone-200" data-cat="烤類">🔥 烤類</button>
        <button onclick="filterCategory('黑胡椒類')" class="tab-btn px-3.5 py-1.5 rounded-full bg-stone-100 hover:bg-stone-200" data-cat="黑胡椒類">🌶️ 黑胡椒類</button>
        <button onclick="filterCategory('香酥類')" class="tab-btn px-3.5 py-1.5 rounded-full bg-stone-100 hover:bg-stone-200" data-cat="香酥類">🍗 香酥類</button>
        <button onclick="filterCategory('滷類')" class="tab-btn px-3.5 py-1.5 rounded-full bg-stone-100 hover:bg-stone-200" data-cat="滷類">🥘 滷類</button>
        <button onclick="filterCategory('飯類')" class="tab-btn px-3.5 py-1.5 rounded-full bg-stone-100 hover:bg-stone-200" data-cat="飯類">🍚 飯類</button>
      </div>
    </nav>
  </header>

  <!-- 用餐方式設定 -->
  <section class="max-w-2xl mx-auto px-4 mt-4">
    <div class="bg-white rounded-xl p-4 shadow-sm border border-stone-200">
      <div class="flex space-x-2 mb-3">
        <button id="btn-takeout" onclick="setOrderType('外帶')" class="flex-1 py-2 rounded-lg font-bold text-sm bg-amber-700 text-white transition">🛍️ 外帶自取</button>
        <button id="btn-dinein" onclick="setOrderType('內用')" class="flex-1 py-2 rounded-lg font-bold text-sm bg-stone-100 text-stone-600 transition">🍽️ 內用點餐</button>
      </div>
      <div id="takeout-fields" class="grid grid-cols-2 gap-2.5">
        <input type="text" id="customer-name" placeholder="訂購人姓名 / 電話末三碼" class="border border-stone-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:border-amber-600">
        <input type="text" id="pickup-time" placeholder="預計自取時間 (如 12:15)" class="border border-stone-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:border-amber-600">
      </div>
      <div id="dinein-fields" class="hidden">
        <input type="text" id="table-number" placeholder="請輸入內用桌號 (例如：3號桌)" class="w-full border border-stone-300 rounded-lg px-3 py-2 text-sm focus:outline-none focus:border-amber-600">
      </div>
    </div>
  </section>

  <!-- 菜單列表 -->
  <main id="menu-container" class="max-w-2xl mx-auto px-4 mt-4 space-y-6"></main>

  <!-- 底部購物車快捷列 -->
  <div id="cart-bar" class="fixed bottom-0 inset-x-0 bg-white border-t border-stone-200 shadow-lg z-30 hidden">
    <div class="max-w-2xl mx-auto px-4 py-3 flex items-center justify-between">
      <div onclick="toggleCartModal()" class="cursor-pointer flex items-center space-x-3">
        <div class="relative bg-amber-800 text-white p-2.5 rounded-full">
          🛒
          <span id="cart-badge" class="absolute -top-1 -right-1 bg-red-600 text-white text-xs font-bold w-5 h-5 rounded-full flex items-center justify-center">0</span>
        </div>
        <div>
          <p class="text-xs text-stone-500">點擊查看明細</p>
          <p class="text-lg font-bold text-stone-900">總計 $<span id="cart-total">0</span></p>
        </div>
      </div>
      <button onclick="toggleCartModal()" class="bg-amber-700 hover:bg-amber-800 text-white font-bold px-6 py-2.5 rounded-xl text-sm shadow">
        查看訂單明細
      </button>
    </div>
  </div>

  <!-- 購物車與結帳明細彈窗 -->
  <div id="cart-modal" class="fixed inset-0 bg-black/50 z-40 hidden flex items-end sm:items-center justify-center">
    <div class="bg-white w-full max-w-lg rounded-t-2xl sm:rounded-2xl max-h-[90vh] flex flex-col overflow-hidden">
      <div class="px-5 py-4 border-b border-stone-200 flex justify-between items-center bg-stone-50">
        <h2 class="font-bold text-lg">確認點餐明細</h2>
        <button onclick="toggleCartModal()" class="text-stone-400 hover:text-stone-700 text-2xl leading-none">&times;</button>
      </div>

      <div id="cart-items-list" class="p-5 overflow-y-auto flex-1 space-y-3"></div>

      <div class="px-5 py-3 border-t border-stone-100 bg-stone-50">
        <label class="block text-xs font-medium text-stone-600 mb-1">整單備註（選填）</label>
        <input type="text" id="order-note" placeholder="例如：飯少、不要菜脯、加辣、需要免洗筷..." class="w-full border border-stone-300 rounded-lg px-3 py-2 text-sm bg-white focus:outline-none focus:border-amber-600">
      </div>

      <div class="p-5 border-t border-stone-200 bg-white space-y-3">
        <div class="flex justify-between items-center text-base font-bold">
          <span>應付總金額</span>
          <span class="text-xl text-amber-800">$<span id="modal-total">0</span></span>
        </div>
        <button onclick="copyOrderText()" class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 rounded-xl text-sm shadow transition flex items-center justify-center space-x-2">
          <span>📋 一鍵複製訂單內容（傳送至 LINE / 出示給店員）</span>
        </button>
        <button onclick="clearCart()" class="w-full text-stone-400 hover:text-red-600 text-xs py-1">清空購物車</button>
      </div>
    </div>
  </div>

  <!-- 複製成功提示 -->
  <div id="toast" class="fixed top-20 left-1/2 -translate-x-1/2 bg-stone-900 text-white text-sm px-4 py-2.5 rounded-full shadow-lg z-50 hidden">
    ✅ 訂單已複製！可直接貼上 LINE 傳送給店家
  </div>

  <script>
    const menuData = [
      { id: 1, category: '烤類', name: '烤肉飯', bentoPrice: 115, soloPrice: 85 },
      { id: 2, category: '烤類', name: '烤排骨飯', bentoPrice: 115, soloPrice: 85 },
      { id: 3, category: '烤類', name: '烤雞排飯', bentoPrice: 120, soloPrice: 95 },
      { id: 4, category: '烤類', name: '烤雞柳飯', bentoPrice: 115, soloPrice: 85 },
      { id: 5, category: '烤類', name: '烤五花肉飯', bentoPrice: 120, soloPrice: 95 },
      { id: 6, category: '烤類', name: '烤鯖魚飯', bentoPrice: 120, soloPrice: 95 },
      { id: 7, category: '烤類', name: '烤雞腿飯', bentoPrice: 120, soloPrice: 95 },

      { id: 8, category: '黑胡椒類', name: '黑胡椒排骨飯', bentoPrice: 115, soloPrice: 85 },
      { id: 9, category: '黑胡椒類', name: '黑胡椒雞排飯', bentoPrice: 120, soloPrice: 95 },
      { id: 10, category: '黑胡椒類', name: '黑胡椒雞柳飯', bentoPrice: 115, soloPrice: 85 },
      { id: 11, category: '黑胡椒類', name: '黑胡椒雞腿飯', bentoPrice: 120, soloPrice: 95 },

      { id: 12, category: '香酥類', name: '香酥魚排飯', bentoPrice: 115, soloPrice: 85 },
      { id: 13, category: '香酥類', name: '香酥排骨飯', bentoPrice: 115, soloPrice: 85 },
      { id: 14, category: '香酥類', name: '香酥雞排飯', bentoPrice: 120, soloPrice: 95 },
      { id: 15, category: '香酥類', name: '香酥雞柳飯', bentoPrice: 115, soloPrice: 85 },
      { id: 16, category: '香酥類', name: '香酥雞腿飯', bentoPrice: 120, soloPrice: 95 },
      { id: 17, category: '香酥類', name: '豪大雞排飯', bentoPrice: 120, soloPrice: 95 },

      { id: 18, category: '滷類', name: '滷雞腿飯', bentoPrice: 120, soloPrice: 95 },

      { id: 19, category: '飯類', name: '白飯', bentoPrice: 15, soloPrice: null },
      { id: 20, category: '飯類', name: '菜飯', bentoPrice: 95, soloPrice: null }
    ];

    let cart = {};
    let currentCategory = 'all';
    let orderType = '外帶';

    function setOrderType(type) {
      orderType = type;
      const btnTakeout = document.getElementById('btn-takeout');
      const btnDinein = document.getElementById('btn-dinein');
      const takeoutFields = document.getElementById('takeout-fields');
      const dineinFields = document.getElementById('dinein-fields');

      if (type === '外帶') {
        btnTakeout.className = 'flex-1 py-2 rounded-lg font-bold text-sm bg-amber-700 text-white transition';
        btnDinein.className = 'flex-1 py-2 rounded-lg font-bold text-sm bg-stone-100 text-stone-600 transition';
        takeoutFields.classList.remove('hidden');
        dineinFields.classList.add('hidden');
      } else {
        btnDinein.className = 'flex-1 py-2 rounded-lg font-bold text-sm bg-amber-700 text-white transition';
        btnTakeout.className = 'flex-1 py-2 rounded-lg font-bold text-sm bg-stone-100 text-stone-600 transition';
        dineinFields.classList.remove('hidden');
        takeoutFields.classList.add('hidden');
      }
    }

    function filterCategory(cat) {
      currentCategory = cat;
      document.querySelectorAll('.tab-btn').forEach(btn => {
        if (btn.dataset.cat === cat) {
          btn.className = 'tab-btn active px-3.5 py-1.5 rounded-full bg-amber-800 text-white';
        } else {
          btn.className = 'tab-btn px-3.5 py-1.5 rounded-full bg-stone-100 hover:bg-stone-200';
        }
      });
      renderMenu();
    }

    function renderMenu() {
      const container = document.getElementById('menu-container');
      container.innerHTML = '';

      const categories = currentCategory === 'all'
