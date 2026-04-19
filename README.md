<!DOCTYPE html>

<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>课程管理系统</title>

  <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <style>
    /* ================================================================
       CSS 变量 — 全局设计令牌
       配色方案：清爽蓝白，告别紫色渐变
    ================================================================ */
    :root {
      --primary:        #2563eb;   /* 蓝色主色 */
      --primary-light:  #3b82f6;
      --primary-dark:   #1d4ed8;
      --primary-bg:     #eff6ff;   /* 蓝色浅背景 */

      --success:        #16a34a;
      --success-bg:     #f0fdf4;
      --danger:         #dc2626;
      --danger-bg:      #fef2f2;
      --warning:        #d97706;
      --warning-bg:     #fffbeb;
      --info:           #0891b2;
      --info-bg:        #ecfeff;

      --gray-50:        #f8fafc;
      --gray-100:       #f1f5f9;
      --gray-200:       #e2e8f0;
      --gray-300:       #cbd5e1;
      --gray-400:       #94a3b8;
      --gray-500:       #64748b;
      --gray-600:       #475569;
      --gray-700:       #334155;
      --gray-800:       #1e293b;
      --gray-900:       #0f172a;

      --bg-page:        #f0f4f8;   /* 页面背景 — 浅蓝灰 */
      --bg-card:        #ffffff;
      --text-primary:   var(--gray-800);
      --text-secondary: var(--gray-500);
      --border:         var(--gray-200);
      --radius-sm:      6px;
      --radius-md:      12px;
      --radius-lg:      18px;
      --shadow-sm:      0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.05);
      --shadow-md:      0 4px 16px rgba(0,0,0,.08), 0 2px 6px rgba(0,0,0,.05);
      --shadow-lg:      0 10px 40px rgba(0,0,0,.10), 0 4px 12px rgba(0,0,0,.06);
      --transition:     all .25s cubic-bezier(.4,0,.2,1);
    }

    /* ================================================================
       全局重置
    ================================================================ */
    *, *::before, *::after {
      margin: 0; padding: 0; box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC',
                   'Microsoft YaHei', sans-serif;
      background: var(--bg-page);
      min-height: 100vh;
      color: var(--text-primary);
      line-height: 1.6;
    }

    /* ================================================================
       页面布局
    ================================================================ */
    .page-wrapper {
      max-width: 1280px;
      margin: 0 auto;
      padding: 32px 24px;
    }

    /* ================================================================
       顶部 Header
    ================================================================ */
    .site-header {
      margin-bottom: 28px;
    }

    .header-top {
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 16px;
      margin-bottom: 20px;
    }

    .header-title {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .header-title .icon-wrap {
      width: 48px;
      height: 48px;
      background: var(--primary);
      border-radius: var(--radius-md);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 22px;
      box-shadow: 0 4px 12px rgba(37,99,235,.35);
    }

    .header-title h1 {
      font-size: 1.75rem;
      font-weight: 700;
      color: var(--gray-900);
      letter-spacing: -.5px;
    }

    .header-title p {
      font-size: .875rem;
      color: var(--text-secondary);
    }

    /* ================================================================
       统计栏 StatsBar
    ================================================================ */
    .stats-bar {
      display: flex;
      gap: 14px;
      flex-wrap: wrap;
    }

    .stat-card {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-md);
      padding: 14px 20px;
      display: flex;
      align-items: center;
      gap: 12px;
      box-shadow: var(--shadow-sm);
      min-width: 140px;
      transition: var(--transition);
    }

    .stat-card:hover {
      box-shadow: var(--shadow-md);
      transform: translateY(-1px);
    }

    .stat-icon {
      width: 36px;
      height: 36px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      flex-shrink: 0;
    }

    .stat-icon.blue   { background: var(--primary-bg); }
    .stat-icon.green  { background: var(--success-bg); }
    .stat-icon.orange { background: var(--warning-bg); }
    .stat-icon.cyan   { background: var(--info-bg);    }
    .stat-icon.red    { background: var(--danger-bg);  }

    .stat-info .stat-value {
      font-size: 1.375rem;
      font-weight: 700;
      color: var(--gray-900);
      line-height: 1;
    }

    .stat-info .stat-label {
      font-size: .75rem;
      color: var(--text-secondary);
      margin-top: 2px;
    }

    /* ================================================================
       控制栏 Controls
    ================================================================ */
    .controls {
      display: flex;
      gap: 12px;
      margin-bottom: 24px;
      flex-wrap: wrap;
      align-items: center;
    }

    .search-wrap {
      flex: 1;
      min-width: 240px;
      position: relative;
    }

    .search-wrap .search-icon-svg {
      position: absolute;
      left: 14px;
      top: 50%;
      transform: translateY(-50%);
      color: var(--gray-400);
      pointer-events: none;
      font-size: 16px;
    }

    .search-input {
      width: 100%;
      padding: 10px 14px 10px 42px;
      border: 1.5px solid var(--border);
      border-radius: var(--radius-md);
      font-size: .9375rem;
      background: var(--bg-card);
      color: var(--text-primary);
      transition: var(--transition);
      outline: none;
    }

    .search-input:focus {
      border-color: var(--primary-light);
      box-shadow: 0 0 0 3px rgba(59,130,246,.12);
    }

    .filter-select {
      padding: 10px 14px;
      border: 1.5px solid var(--border);
      border-radius: var(--radius-md);
      font-size: .9375rem;
      background: var(--bg-card);
      color: var(--text-primary);
      cursor: pointer;
      transition: var(--transition);
      outline: none;
      min-width: 140px;
    }

    .filter-select:focus {
      border-color: var(--primary-light);
      box-shadow: 0 0 0 3px rgba(59,130,246,.12);
    }

    .add-btn {
      display: flex;
      align-items: center;
      gap: 7px;
      padding: 10px 22px;
      background: var(--primary);
      color: white;
      border: none;
      border-radius: var(--radius-md);
      font-size: .9375rem;
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
      white-space: nowrap;
      box-shadow: 0 2px 8px rgba(37,99,235,.30);
    }

    .add-btn:hover {
      background: var(--primary-dark);
      transform: translateY(-1px);
      box-shadow: 0 4px 14px rgba(37,99,235,.40);
    }

    .add-btn:active {
      transform: translateY(0);
    }

    /* 搜索结果提示 */
    .search-hint {
      font-size: .8125rem;
      color: var(--text-secondary);
      margin-bottom: 16px;
      padding-left: 2px;
    }

    .search-hint strong {
      color: var(--primary);
      font-weight: 600;
    }

    /* ================================================================
       课程网格
    ================================================================ */
    .course-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 18px;
    }

    /* ================================================================
       课程卡片 CourseCard
    ================================================================ */
    .course-card {
      background: var(--bg-card);
      border: 1px solid var(--border);
      border-radius: var(--radius-lg);
      padding: 22px;
      box-shadow: var(--shadow-sm);
      transition: var(--transition);
      position: relative;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }

    .course-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 3px;
      border-radius: var(--radius-lg) var(--radius-lg) 0 0;
    }

    .course-card.studying {
      animation: studyPulse 1.5s ease-in-out infinite;
    }

    @keyframes studyPulse {
      0%, 100% { box-shadow: var(--shadow-sm); }
      50%       { box-shadow: 0 0 0 4px rgba(37,99,235,.15), var(--shadow-md); }
    }

    .course-card:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-lg);
      border-color: var(--gray-300);
    }

    /* 分类色条 + 标签 */
    .card-cat-frontend  { --cat-color: #2563eb; }
    .card-cat-backend   { --cat-color: #0891b2; }
    .card-cat-database  { --cat-color: #16a34a; }
    .card-cat-devops    { --cat-color: #d97706; }
    .card-cat-default   { --cat-color: #6d28d9; }

    .course-card::before { background: var(--cat-color, #2563eb); }

    .category-tag {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      padding: 3px 11px;
      border-radius: 20px;
      font-size: .75rem;
      font-weight: 600;
      letter-spacing: .2px;
      margin-bottom: 10px;
      background: color-mix(in srgb, var(--cat-color, #2563eb) 12%, white);
      color: var(--cat-color, #2563eb);
      border: 1px solid color-mix(in srgb, var(--cat-color, #2563eb) 25%, white);
    }

    .course-name {
      font-size: 1.1rem;
      font-weight: 700;
      color: var(--gray-900);
      margin-bottom: 8px;
      line-height: 1.4;
    }

    .course-description {
      font-size: .875rem;
      color: var(--gray-500);
      line-height: 1.65;
      flex: 1;
      margin-bottom: 16px;
    }

    /* 操作按钮 */
    .course-actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    .btn {
      padding: 7px 16px;
      border: none;
      border-radius: var(--radius-sm);
      font-size: .8125rem;
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
      display: inline-flex;
      align-items: center;
      gap: 5px;
    }

    .btn-study {
      background: var(--primary-bg);
      color: var(--primary);
      border: 1.5px solid color-mix(in srgb, var(--primary) 20%, white);
    }

    .btn-study:hover {
      background: var(--primary);
      color: white;
    }

    .btn-study.is-studying {
      background: var(--primary);
      color: white;
      cursor: default;
      opacity: .85;
    }

    .btn-edit {
      background: var(--warning-bg);
      color: var(--warning);
      border: 1.5px solid color-mix(in srgb, var(--warning) 20%, white);
    }

    .btn-edit:hover {
      background: var(--warning);
      color: white;
    }

    .btn-delete {
      background: var(--danger-bg);
      color: var(--danger);
      border: 1.5px solid color-mix(in srgb, var(--danger) 20%, white);
    }

    .btn-delete:hover {
      background: var(--danger);
      color: white;
    }

    /* ================================================================
       空状态
    ================================================================ */
    .empty-state {
      grid-column: 1 / -1;
      text-align: center;
      padding: 72px 24px;
      color: var(--text-secondary);
    }

    .empty-state .empty-icon {
      font-size: 3.5rem;
      margin-bottom: 16px;
      opacity: .5;
    }

    .empty-state h3 {
      font-size: 1.25rem;
      font-weight: 600;
      color: var(--gray-600);
      margin-bottom: 8px;
    }

    .empty-state p {
      font-size: .9rem;
      color: var(--gray-400);
    }

    /* ================================================================
       模态框
    ================================================================ */
    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(15,23,42,.45);
      backdrop-filter: blur(4px);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1000;
      padding: 20px;
    }

    .modal {
      background: var(--bg-card);
      border-radius: var(--radius-lg);
      width: 100%;
      max-width: 520px;
      box-shadow: var(--shadow-lg);
      animation: modalIn .22s cubic-bezier(.34,1.3,.64,1);
      overflow: hidden;
    }

    @keyframes modalIn {
      from { opacity: 0; transform: scale(.94) translateY(8px); }
      to   { opacity: 1; transform: scale(1) translateY(0);     }
    }

    .modal-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 22px 26px 18px;
      border-bottom: 1px solid var(--border);
    }

    .modal-header h2 {
      font-size: 1.125rem;
      font-weight: 700;
      color: var(--gray-900);
    }

    .modal-close {
      width: 30px;
      height: 30px;
      border: none;
      background: var(--gray-100);
      border-radius: 6px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      color: var(--gray-500);
      transition: var(--transition);
    }

    .modal-close:hover {
      background: var(--gray-200);
      color: var(--gray-800);
    }

    .modal-body {
      padding: 24px 26px;
    }

    /* ================================================================
       表单
    ================================================================ */
    .form-group {
      margin-bottom: 18px;
    }

    .form-group label {
      display: block;
      margin-bottom: 6px;
      font-size: .875rem;
      font-weight: 600;
      color: var(--gray-700);
    }

    .form-group label .required {
      color: var(--danger);
      margin-left: 2px;
    }

    .form-control {
      width: 100%;
      padding: 10px 14px;
      border: 1.5px solid var(--border);
      border-radius: var(--radius-sm);
      font-size: .9375rem;
      color: var(--text-primary);
      background: var(--bg-card);
      transition: var(--transition);
      outline: none;
      font-family: inherit;
    }

    .form-control:focus {
      border-color: var(--primary-light);
      box-shadow: 0 0 0 3px rgba(59,130,246,.12);
    }

    .form-control.is-error {
      border-color: var(--danger);
      box-shadow: 0 0 0 3px rgba(220,38,38,.10);
    }

    textarea.form-control {
      resize: vertical;
      min-height: 96px;
    }

    .form-error {
      display: flex;
      align-items: center;
      gap: 4px;
      color: var(--danger);
      font-size: .8rem;
      margin-top: 5px;
    }

    .modal-footer {
      display: flex;
      gap: 10px;
      justify-content: flex-end;
      padding: 16px 26px 22px;
      border-top: 1px solid var(--border);
    }

    .btn-cancel {
      background: var(--gray-100);
      color: var(--gray-600);
      border: 1.5px solid var(--gray-200);
    }

    .btn-cancel:hover {
      background: var(--gray-200);
    }

    .btn-submit {
      background: var(--primary);
      color: white;
      border: 1.5px solid var(--primary-dark);
      box-shadow: 0 2px 8px rgba(37,99,235,.28);
    }

    .btn-submit:hover {
      background: var(--primary-dark);
    }

    /* ================================================================
       Toast 通知
    ================================================================ */
    .toast-wrap {
      position: fixed;
      bottom: 28px;
      right: 28px;
      z-index: 2000;
      display: flex;
      flex-direction: column;
      gap: 10px;
      pointer-events: none;
    }

    .toast {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 13px 20px;
      border-radius: var(--radius-md);
      box-shadow: var(--shadow-lg);
      font-size: .9rem;
      font-weight: 500;
      min-width: 240px;
      max-width: 360px;
      animation: toastIn .28s cubic-bezier(.34,1.3,.64,1);
      pointer-events: all;
    }

    @keyframes toastIn {
      from { opacity: 0; transform: translateX(60px); }
      to   { opacity: 1; transform: translateX(0);     }
    }

    .toast.success {
      background: #dcfce7;
      color: #15803d;
      border: 1px solid #bbf7d0;
    }

    .toast.error {
      background: #fee2e2;
      color: #b91c1c;
      border: 1px solid #fecaca;
    }

    .toast.info {
      background: #dbeafe;
      color: #1d4ed8;
      border: 1px solid #bfdbfe;
    }

    .toast-icon { font-size: 1.1em; }

    /* ================================================================
       删除确认弹窗（内联确认，代替 confirm()）
    ================================================================ */
    .confirm-overlay {
      position: fixed;
      inset: 0;
      background: rgba(15,23,42,.4);
      backdrop-filter: blur(3px);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1100;
    }

    .confirm-box {
      background: var(--bg-card);
      border-radius: var(--radius-lg);
      padding: 28px 32px;
      max-width: 360px;
      width: 90%;
      box-shadow: var(--shadow-lg);
      text-align: center;
      animation: modalIn .22s cubic-bezier(.34,1.3,.64,1);
    }

    .confirm-box .confirm-icon { font-size: 2.5rem; margin-bottom: 12px; }
    .confirm-box h3 { font-size: 1.1rem; font-weight: 700; color: var(--gray-900); margin-bottom: 8px; }
    .confirm-box p  { font-size: .875rem; color: var(--gray-500); margin-bottom: 22px; }
    .confirm-actions { display: flex; gap: 10px; justify-content: center; }

    /* ================================================================
       响应式
    ================================================================ */
    @media (max-width: 640px) {
      .page-wrapper { padding: 16px; }
      .header-top   { flex-direction: column; align-items: flex-start; }
      .stats-bar    { gap: 8px; }
      .stat-card    { min-width: 110px; padding: 10px 14px; }
      .course-grid  { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
<div id="root"></div>

<script type="text/babel">
  /*
   * ================================================================
   * 解构所有需要的 React Hooks
   * ================================================================
   */
  const { useState, useEffect, useRef, useMemo, useCallback } = React;

  /* ==============================================================
   * 自定义 Hook #1: useLocalStorage
   * ——————————————————————————————————————————————————————————————
   * 封装 localStorage 读写逻辑，让状态自动与本地存储双向同步
   * 技术点：
   *   - 惰性初始化（useState 传入初始化函数）
   *   - useEffect 依赖数组 [key, value]
   *   - JSON 序列化 / 反序列化
   * ============================================================== */
  function useLocalStorage(key, initialValue) {
    // 惰性初始化：只在首次渲染时执行，读取 localStorage
    const [storedValue, setStoredValue] = useState(() => {
      try {
        const item = window.localStorage.getItem(key);
        return item ? JSON.parse(item) : initialValue;
      } catch (err) {
        console.warn('useLocalStorage 读取失败:', err);
        return initialValue;
      }
    });

    // useEffect：监听 storedValue 变化，自动同步到 localStorage
    useEffect(() => {
      try {
        window.localStorage.setItem(key, JSON.stringify(storedValue));
      } catch (err) {
        console.warn('useLocalStorage 写入失败:', err);
      }
    }, [key, storedValue]);

    return [storedValue, setStoredValue];
  }

  /* ==============================================================
   * 自定义 Hook #2: useDebounce
   * ——————————————————————————————————————————————————————————————
   * 对高频输入值进行防抖处理，减少不必要的过滤计算
   * 技术点：
   *   - useEffect + setTimeout + clearTimeout 清理
   *   - 闭包捕获 value
   * ============================================================== */
  function useDebounce(value, delay = 300) {
    const [debouncedValue, setDebouncedValue] = useState(value);

    useEffect(() => {
      const timer = setTimeout(() => setDebouncedValue(value), delay);
      // 清理函数：每次 value/delay 变化时清除上一个定时器
      return () => clearTimeout(timer);
    }, [value, delay]);

    return debouncedValue;
  }

  /* ==============================================================
   * 工具：根据分类名返回 CSS 类名
   * ============================================================== */
  const CATEGORY_CLASS_MAP = {
    '前端开发': 'frontend',
    '后端开发': 'backend',
    '数据库':   'database',
    '运维部署': 'devops',
  };

  const CATEGORY_ICONS = {
    '前端开发': '🌐',
    '后端开发': '⚙️',
    '数据库':   '🗄️',
    '运维部署': '🚀',
  };

  const STAT_COLORS = ['blue', 'green', 'cyan', 'orange', 'red'];
  const STAT_ICONS  = ['📚', '🌐', '⚙️', '🗄️', '🚀'];

  /* ==============================================================
   * 默认课程数据
   * ============================================================== */
  const DEFAULT_COURSES = [
    { id: 1, name: 'React 入门到精通',    description: '学习React框架的核心概念，包括组件、状态管理、Hooks生命周期等知识。',      category: '前端开发' },
    { id: 2, name: 'Node.js 后端开发',    description: '掌握Node.js服务端开发，构建高性能的RESTful API与Web应用程序。',          category: '后端开发' },
    { id: 3, name: 'MySQL 数据库设计',    description: '学习关系型数据库设计原理，掌握SQL查询优化与索引技巧。',                   category: '数据库'   },
    { id: 4, name: 'Docker 容器化部署',   description: '了解容器化技术，学习使用Docker与Kubernetes进行应用部署和管理。',          category: '运维部署' },
    { id: 5, name: 'TypeScript 进阶',     description: '深入TypeScript类型系统，使用泛型、装饰器构建企业级可维护前端项目。',       category: '前端开发' },
  ];

  /* ==============================================================
   * Toast 通知组件
   * ——————————————————————————————————————————————————————————————
   * Props: message / type / onClose
   * 技术点：useEffect 定时自动关闭，返回清理函数
   * ============================================================== */
  function Toast({ message, type, onClose }) {
    useEffect(() => {
      const timer = setTimeout(onClose, 3200);
      return () => clearTimeout(timer);
    }, [onClose]);

    const icons = { success: '✅', error: '❌', info: 'ℹ️' };
    return (
      <div className={`toast ${type}`}>
        <span className="toast-icon">{icons[type] || 'ℹ️'}</span>
        {message}
      </div>
    );
  }

  /* ==============================================================
   * ConfirmDialog 内联确认弹窗（替代浏览器 confirm()）
   * Props: message / onConfirm / onCancel
   * ============================================================== */
  function ConfirmDialog({ message, onConfirm, onCancel }) {
    return (
      <div className="confirm-overlay" onClick={onCancel}>
        <div className="confirm-box" onClick={e => e.stopPropagation()}>
          <div className="confirm-icon">⚠️</div>
          <h3>确认删除</h3>
          <p>{message}</p>
          <div className="confirm-actions">
            <button className="btn btn-cancel" onClick={onCancel}>取消</button>
            <button className="btn btn-delete" onClick={onConfirm}>确认删除</button>
          </div>
        </div>
      </div>
    );
  }

  /* ==============================================================
   * StatsBar 统计栏组件
   * ——————————————————————————————————————————————————————————————
   * Props: courses(课程列表), categories(分类数组)
   * 技术点：
   *   - props 接收数据
   *   - 数组 filter + length 统计
   *   - map 列表渲染
   * ============================================================== */
  function StatsBar({ courses, categories }) {
    const total = courses.length;
    const catStats = categories.map((cat, i) => ({
      label: cat,
      count: courses.filter(c => c.category === cat).length,
      color: STAT_COLORS[i + 1] || 'blue',
      icon:  STAT_ICONS[i + 1]  || '📖',
    }));

    return (
      <div className="stats-bar">
        {/* 总数卡片 */}
        <div className="stat-card">
          <div className="stat-icon blue">📚</div>
          <div className="stat-info">
            <div className="stat-value">{total}</div>
            <div className="stat-label">课程总数</div>
          </div>
        </div>
        {/* 各分类统计 */}
        {catStats.map(s => (
          <div className="stat-card" key={s.label}>
            <div className={`stat-icon ${s.color}`}>{s.icon}</div>
            <div className="stat-info">
              <div className="stat-value">{s.count}</div>
              <div className="stat-label">{s.label}</div>
            </div>
          </div>
        ))}
      </div>
    );
  }

  /* ==============================================================
   * SearchBar 搜索/筛选栏组件
   * ——————————————————————————————————————————————————————————————
   * Props: searchTerm / onSearch / filterCategory / onFilter /
   *        categories / onAdd / inputRef
   * 技术点：
   *   - useRef 转发（inputRef 来自父组件，实现自动聚焦）
   *   - 受控组件（value + onChange）
   *   - props 事件回调
   * ============================================================== */
  function SearchBar({ searchTerm, onSearch, filterCategory, onFilter, categories, onAdd, inputRef }) {
    return (
      <div className="controls">
        {/* 搜索框 — 受控组件 + useRef 转发 */}
        <div className="search-wrap">
          <span className="search-icon-svg">🔍</span>
          <input
            ref={inputRef}
            className="search-input"
            type="text"
            placeholder="搜索课程名称或简介..."
            value={searchTerm}
            onChange={e => onSearch(e.target.value)}
          />
        </div>

        {/* 分类筛选 — 受控组件 */}
        <select
          className="filter-select"
          value={filterCategory}
          onChange={e => onFilter(e.target.value)}
        >
          <option value="全部">全部分类</option>
          {categories.map(cat => (
            <option key={cat} value={cat}>{cat}</option>
          ))}
        </select>

        {/* 添加按钮 */}
        <button className="add-btn" onClick={onAdd}>
          <span>＋</span> 添加课程
        </button>
      </div>
    );
  }

  /* ==============================================================
   * CourseCard 课程卡片组件
   * ——————————————————————————————————————————————————————————————
   * Props: course / onStudy / onEdit / onDelete
   * 技术点：
   *   - useState 管理 isStudying
   *   - useCallback 接收来自父组件的优化回调
   *   - 条件渲染（学习中 vs 开始学习）
   *   - 动态 className
   * ============================================================== */
  function CourseCard({ course, onStudy, onEdit, onDelete }) {
    const [isStudying, setIsStudying] = useState(false);

    // 本地学习按钮处理（不需要 useCallback，因为不会作为 prop 传给子组件）
    const handleStudyClick = () => {
      if (isStudying) return;
      setIsStudying(true);
      onStudy(course.name);
      setTimeout(() => setIsStudying(false), 2200);
    };

    const catClass  = CATEGORY_CLASS_MAP[course.category] || 'default';
    const catIcon   = CATEGORY_ICONS[course.category]    || '📖';

    return (
      <div className={`course-card card-cat-${catClass} ${isStudying ? 'studying' : ''}`}>
        {/* 分类标签 */}
        <span className={`category-tag`}>
          {catIcon} {course.category}
        </span>

        {/* 课程标题 */}
        <h3 className="course-name">{course.name}</h3>

        {/* 课程简介 */}
        <p className="course-description">{course.description}</p>

        {/* 操作按钮区 */}
        <div className="course-actions">
          <button
            className={`btn btn-study ${isStudying ? 'is-studying' : ''}`}
            onClick={handleStudyClick}
          >
            {isStudying ? '⏳ 学习中...' : '▶ 开始学习'}
          </button>
          <button className="btn btn-edit"   onClick={() => onEdit(course)}>✏️ 编辑</button>
          <button className="btn btn-delete" onClick={() => onDelete(course.id)}>🗑 删除</button>
        </div>
      </div>
    );
  }

  /* ==============================================================
   * CourseForm 课程表单组件
   * ——————————————————————————————————————————————————————————————
   * Props: course(编辑时传入) / onSubmit / onCancel / categories /
   *        firstInputRef(useRef 转发，实现自动聚焦)
   * 技术点：
   *   - 受控组件（formData 对象）
   *   - 表单验证 validateForm
   *   - useEffect: 首次渲染完成后聚焦到课程名输入框
   * ============================================================== */
  function CourseForm({ course, onSubmit, onCancel, categories, firstInputRef }) {
    const [formData, setFormData] = useState({
      name:        course?.name        || '',
      description: course?.description || '',
      category:    course?.category    || '前端开发',
    });
    const [errors, setErrors] = useState({});

    // useEffect: 表单挂载后，通过 ref 聚焦到名称输入框
    useEffect(() => {
      if (firstInputRef?.current) {
        firstInputRef.current.focus();
      }
    }, []);

    // 表单验证
    const validate = () => {
      const newErrors = {};
      if (!formData.name.trim())           newErrors.name = '课程名称不能为空';
      else if (formData.name.trim().length < 2) newErrors.name = '名称至少需要 2 个字符';
      if (!formData.description.trim())    newErrors.description = '课程简介不能为空';
      setErrors(newErrors);
      return Object.keys(newErrors).length === 0;
    };

    const handleSubmit = (e) => {
      e.preventDefault();
      if (validate()) onSubmit(formData);
    };

    const handleChange = (e) => {
      const { name, value } = e.target;
      setFormData(prev => ({ ...prev, [name]: value }));
      if (errors[name]) setErrors(prev => ({ ...prev, [name]: '' }));
    };

    return (
      <form onSubmit={handleSubmit}>
        {/* 课程名称 */}
        <div className="form-group">
          <label>课程名称 <span className="required">*</span></label>
          <input
            ref={firstInputRef}
            className={`form-control ${errors.name ? 'is-error' : ''}`}
            type="text"
            name="name"
            value={formData.name}
            onChange={handleChange}
            placeholder="请输入课程名称"
          />
          {errors.name && <div className="form-error">⚠ {errors.name}</div>}
        </div>

        {/* 课程简介 */}
        <div className="form-group">
          <label>课程简介 <span className="required">*</span></label>
          <textarea
            className={`form-control ${errors.description ? 'is-error' : ''}`}
            name="description"
            value={formData.description}
            onChange={handleChange}
            placeholder="请输入课程简介"
          />
          {errors.description && <div className="form-error">⚠ {errors.description}</div>}
        </div>

        {/* 课程分类 */}
        <div className="form-group">
          <label>课程分类</label>
          <select
            className="form-control"
            name="category"
            value={formData.category}
            onChange={handleChange}
          >
            {categories.map(cat => (
              <option key={cat} value={cat}>{cat}</option>
            ))}
          </select>
        </div>
      </form>
    );
  }

  /* ==============================================================
   * Modal 模态框组件
   * ——————————————————————————————————————————————————————————————
   * Props: title / children / onClose / onConfirm
   * 技术点：
   *   - children 插槽
   *   - 阻止事件冒泡
   * ============================================================== */
  function Modal({ title, children, onClose, onConfirm, confirmLabel = '确认' }) {
    return (
      <div className="modal-overlay" onClick={onClose}>
        <div className="modal" onClick={e => e.stopPropagation()}>
          <div className="modal-header">
            <h2>{title}</h2>
            <button className="modal-close" onClick={onClose}>✕</button>
          </div>
          <div className="modal-body">
            {children}
          </div>
          <div className="modal-footer">
            <button className="btn btn-cancel" onClick={onClose}>取消</button>
            <button className="btn btn-submit" onClick={onConfirm}>{confirmLabel}</button>
          </div>
        </div>
      </div>
    );
  }

  /* ==============================================================
   * App 主应用组件
   * ——————————————————————————————————————————————————————————————
   * 核心技术点：
   *   - useLocalStorage 自定义 Hook 持久化课程数据
   *   - useState 管理 UI 状态
   *   - useRef 实现表单输入框自动聚焦
   *   - useMemo 缓存过滤后的课程列表
   *   - useCallback 封装事件处理函数
   *   - useDebounce 自定义 Hook 防抖搜索
   * ============================================================== */
  function App() {
    const CATEGORIES = ['前端开发', '后端开发', '数据库', '运维部署'];

    /* ---------- 状态 ---------- */
    // useLocalStorage 自定义 Hook：持久化课程列表
    const [courses, setCourses] = useLocalStorage('courses_v2', DEFAULT_COURSES);

    const [searchTerm,      setSearchTerm]      = useState('');
    const [filterCategory,  setFilterCategory]  = useState('全部');
    const [showModal,       setShowModal]        = useState(false);
    const [editingCourse,   setEditingCourse]    = useState(null);
    const [toastList,       setToastList]        = useState([]);
    const [confirmTarget,   setConfirmTarget]    = useState(null); // 待确认删除的课程 id

    /* ---------- useRef ---------- */
    // useRef: 引用表单内"课程名称"输入框，弹窗打开后自动聚焦
    const formNameRef = useRef(null);
    // useRef: 用于强制重新挂载 CourseForm，实现添加后重置表单
    const formKeyRef = useRef(0);

    /* ---------- useDebounce ---------- */
    // 对搜索词防抖，300ms 后才触发过滤，降低计算频率
    const debouncedSearch = useDebounce(searchTerm, 300);

    /* ---------- useMemo ---------- */
    // useMemo: 仅当 courses / debouncedSearch / filterCategory 变化时重新计算
    // 避免每次渲染都重复过滤
    const filteredCourses = useMemo(() => {
      return courses.filter(course => {
        const term = debouncedSearch.toLowerCase();
        const matchesSearch =
          course.name.toLowerCase().includes(term) ||
          course.description.toLowerCase().includes(term);
        const matchesCategory =
          filterCategory === '全部' || course.category === filterCategory;
        return matchesSearch && matchesCategory;
      });
    }, [courses, debouncedSearch, filterCategory]);

    /* ---------- 工具函数 ---------- */
    const pushToast = useCallback((message, type = 'info') => {
      const id = Date.now();
      setToastList(prev => [...prev, { id, message, type }]);
      // Toast 组件自行定时关闭，这里不需要 setTimeout
    }, []);

    const removeToast = useCallback((id) => {
      setToastList(prev => prev.filter(t => t.id !== id));
    }, []);

    /* ---------- useCallback 封装事件处理函数 ---------- */
    // useCallback: 依赖稳定，避免因父组件重渲染导致子组件 prop 变化

    // 添加课程
    const handleAddCourse = useCallback((formData) => {
      const newCourse = { id: Date.now(), ...formData };
      setCourses(prev => [...prev, newCourse]);
      pushToast('课程添加成功！继续添加下一条', 'success');
      // 添加成功后：递增 key 强制重新挂载表单，清空输入框并自动聚焦
      formKeyRef.current += 1;
      // 强制重新渲染以应用新的 key
      setEditingCourse(null);
    }, [setCourses, pushToast]);

    // 保存编辑
    const handleEditCourse = useCallback((formData) => {
      setCourses(prev =>
        prev.map(c => c.id === editingCourse.id ? { ...c, ...formData } : c)
      );
      setShowModal(false);
      setEditingCourse(null);
      pushToast('课程修改成功！', 'success');
    }, [editingCourse, setCourses, pushToast]);

    // 删除课程（先弹确认弹窗）
    const handleDeleteCourse = useCallback((id) => {
      setConfirmTarget(id);
    }, []);

    // 确认删除
    const confirmDelete = useCallback(() => {
      setCourses(prev => prev.filter(c => c.id !== confirmTarget));
      setConfirmTarget(null);
      pushToast('课程已删除', 'error');
    }, [confirmTarget, setCourses, pushToast]);

    // 打开编辑模态框
    const openEditModal = useCallback((course) => {
      setEditingCourse(course);
      setShowModal(true);
    }, []);

    // 打开添加模态框
    const openAddModal = useCallback(() => {
      setEditingCourse(null);
      setShowModal(true);
    }, []);

    // 学习课程
    const handleStudy = useCallback((courseName) => {
      pushToast(`📖 正在学习：${courseName}`, 'info');
    }, [pushToast]);

    // 关闭模态框
    const closeModal = useCallback(() => {
      setShowModal(false);
      setEditingCourse(null);
    }, []);

    // 模态框"确认"按钮 — 触发表单提交
    // （Modal 内没有 <form>，需要在 onConfirm 中手动触发表单提交）
    const handleModalConfirm = useCallback(() => {
      // 通过事件代理触发 CourseForm 的 form submit
      const form = document.getElementById('course-form-inner');
      if (form) form.dispatchEvent(new Event('submit', { bubbles: true, cancelable: true }));
    }, []);

    /* ---------- JSX ---------- */
    return (
      <div className="page-wrapper">

        {/* ====== 页面头部 ====== */}
        <header className="site-header">
          <div className="header-top">
            <div className="header-title">
              <div className="icon-wrap">📚</div>
              <div>
                <h1>课程管理系统</h1>
                <p>管理你的学习课程，持续精进</p>
              </div>
            </div>
          </div>

          {/* 统计栏 */}
          <StatsBar courses={courses} categories={CATEGORIES} />
        </header>

        {/* ====== 搜索 / 筛选 / 添加 ====== */}
        <SearchBar
          searchTerm={searchTerm}
          onSearch={setSearchTerm}
          filterCategory={filterCategory}
          onFilter={setFilterCategory}
          categories={CATEGORIES}
          onAdd={openAddModal}
          inputRef={null}
        />

        {/* 搜索结果提示 */}
        {(debouncedSearch || filterCategory !== '全部') && (
          <p className="search-hint">
            找到 <strong>{filteredCourses.length}</strong> 门课程
            {debouncedSearch && <>（关键词：<strong>{debouncedSearch}</strong>）</>}
            {filterCategory !== '全部' && <>（分类：<strong>{filterCategory}</strong>）</>}
          </p>
        )}

        {/* ====== 课程卡片列表 ====== */}
        <div className="course-grid">
          {filteredCourses.length > 0 ? (
            filteredCourses.map(course => (
              <CourseCard
                key={course.id}
                course={course}
                onStudy={handleStudy}
                onEdit={openEditModal}
                onDelete={handleDeleteCourse}
              />
            ))
          ) : (
            <div className="empty-state">
              <div className="empty-icon">🔍</div>
              <h3>{debouncedSearch ? '没有找到匹配的课程' : '暂无课程'}</h3>
              <p>
                {debouncedSearch
                  ? `换个关键词试试，或 点击"添加课程" 新建一门课`
                  : '点击右上角"添加课程"按钮开始创建'}
              </p>
            </div>
          )}
        </div>

        {/* ====== 添加 / 编辑 模态框 ====== */}
        {showModal && (
          <Modal
            title={editingCourse ? '✏️ 编辑课程' : '➕ 添加新课程'}
            onClose={closeModal}
            onConfirm={handleModalConfirm}
            confirmLabel={editingCourse ? '保存修改' : '添加课程'}
          >
            {/*
              CourseForm 内嵌一个 id="course-form-inner" 的 <form>
              onSubmit 根据 editingCourse 决定调用 add 还是 edit
            */}
            <CourseFormWrapper
              key={`${editingCourse?.id ?? 'new'}-${formKeyRef.current}`}
              course={editingCourse}
              onSubmit={editingCourse ? handleEditCourse : handleAddCourse}
              onCancel={closeModal}
              categories={CATEGORIES}
              firstInputRef={formNameRef}
            />
          </Modal>
        )}

        {/* ====== 删除确认弹窗 ====== */}
        {confirmTarget !== null && (
          <ConfirmDialog
            message={`确定要删除「${courses.find(c => c.id === confirmTarget)?.name}」吗？此操作无法撤销。`}
            onConfirm={confirmDelete}
            onCancel={() => setConfirmTarget(null)}
          />
        )}

        {/* ====== Toast 通知堆叠 ====== */}
        <div className="toast-wrap">
          {toastList.map(t => (
            <Toast
              key={t.id}
              message={t.message}
              type={t.type}
              onClose={() => removeToast(t.id)}
            />
          ))}
        </div>
      </div>
    );
  }

  /* ==============================================================
   * CourseFormWrapper
   * ——————————————————————————————————————————————————————————————
   * 将 CourseForm 包裹在一个带 id 的 <form> 内，
   * 便于 Modal 的"确认"按钮通过 dispatchEvent 触发提交
   * ============================================================== */
  function CourseFormWrapper({ course, onSubmit, onCancel, categories, firstInputRef }) {
    const [formData, setFormData] = useState({
      name:        course?.name        || '',
      description: course?.description || '',
      category:    course?.category    || '前端开发',
    });
    const [errors, setErrors] = useState({});

    // useEffect + useRef: 表单挂载后自动聚焦到课程名称输入框
    useEffect(() => {
      if (firstInputRef?.current) {
        firstInputRef.current.focus();
      }
    }, []);

    const validate = () => {
      const newErrors = {};
      if (!formData.name.trim())                newErrors.name = '课程名称不能为空';
      else if (formData.name.trim().length < 2) newErrors.name = '名称至少需要 2 个字符';
      if (!formData.description.trim())         newErrors.description = '课程简介不能为空';
      setErrors(newErrors);
      return Object.keys(newErrors).length === 0;
    };

    const handleSubmit = (e) => {
      e.preventDefault();
      if (validate()) onSubmit(formData);
    };

    const handleChange = (e) => {
      const { name, value } = e.target;
      setFormData(prev => ({ ...prev, [name]: value }));
      if (errors[name]) setErrors(prev => ({ ...prev, [name]: '' }));
    };

    return (
      <form id="course-form-inner" onSubmit={handleSubmit}>
        {/* 课程名称 */}
        <div className="form-group">
          <label>课程名称 <span className="required">*</span></label>
          <input
            ref={firstInputRef}
            className={`form-control ${errors.name ? 'is-error' : ''}`}
            type="text"
            name="name"
            value={formData.name}
            onChange={handleChange}
            placeholder="请输入课程名称"
          />
          {errors.name && <div className="form-error">⚠ {errors.name}</div>}
        </div>

        {/* 课程简介 */}
        <div className="form-group">
          <label>课程简介 <span className="required">*</span></label>
          <textarea
            className={`form-control ${errors.description ? 'is-error' : ''}`}
            name="description"
            value={formData.description}
            onChange={handleChange}
            placeholder="请输入课程简介，描述本课程的主要内容和学习目标"
          />
          {errors.description && <div className="form-error">⚠ {errors.description}</div>}
        </div>

        {/* 课程分类 */}
        <div className="form-group">
          <label>课程分类</label>
          <select
            className="form-control"
            name="category"
            value={formData.category}
            onChange={handleChange}
          >
            {categories.map(cat => (
              <option key={cat} value={cat}>{cat}</option>
            ))}
          </select>
        </div>
      </form>
    );
  }

  /* ================================================================
     渲染入口
  ================================================================ */
  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(<App />);
</script>
</body>
</html>
