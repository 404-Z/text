<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>培训机构学员签到系统</title>
    <!-- Tailwind CSS v3 -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome -->
    <link href="https://cdn.jsdelivr.net/npm/font-awesome@4.7.0/css/font-awesome.min.css" rel="stylesheet">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.8/dist/chart.umd.min.js"></script>
    <!-- Tailwind 配置 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#3b82f6',
                        secondary: '#64748b',
                        success: '#10b981',
                        warning: '#f59e0b',
                        danger: '#ef4444',
                        info: '#06b6d4',
                        light: '#f3f4f6',
                        dark: '#1f2937',
                    },
                    fontFamily: {
                        sans: ['Inter', 'system-ui', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .transition-all-300 {
                transition: all 300ms ease-in-out;
            }
            .shadow-hover:hover {
                box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            }
        }
    </style>
</head>
<body class="bg-gray-50 font-sans">
    <div class="flex h-screen overflow-hidden">
        <!-- 侧边导航栏 -->
        <aside class="bg-dark text-white w-64 flex-shrink-0 hidden md:block">
            <div class="p-4 border-b border-gray-700">
                <h1 class="text-xl font-bold">学员签到系统</h1>
            </div>
            <nav class="mt-5">
                <ul>
                    <li>
                        <a href="#dashboard" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 nav-link active" data-target="dashboard">
                            <i class="fa fa-dashboard mr-3"></i>
                            <span>仪表盘</span>
                        </a>
                    </li>
                    <li>
                        <a href="#class-management" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 nav-link" data-target="class-management">
                            <i class="fa fa-users mr-3"></i>
                            <span>班级管理</span>
                        </a>
                    </li>
                    <li>
                        <a href="#student-management" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 nav-link" data-target="student-management">
                            <i class="fa fa-user mr-3"></i>
                            <span>学员管理</span>
                        </a>
                    </li>
                    <li>
                        <a href="#attendance" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 nav-link" data-target="attendance">
                            <i class="fa fa-check-square-o mr-3"></i>
                            <span>签到管理</span>
                        </a>
                    </li>
                    <li>
                        <a href="#statistics" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 nav-link" data-target="statistics">
                            <i class="fa fa-bar-chart mr-3"></i>
                            <span>统计报表</span>
                        </a>
                    </li>
                </ul>
            </nav>
            <div class="absolute bottom-0 w-64 p-4 border-t border-gray-700">
                <button id="export-data" class="flex items-center justify-center w-full px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                    <i class="fa fa-download mr-2"></i>
                    <span>导出数据</span>
                </button>
                <button id="import-data" class="flex items-center justify-center w-full px-4 py-2 mt-2 bg-secondary text-white rounded hover:bg-gray-600 transition-all-300">
                    <i class="fa fa-upload mr-2"></i>
                    <span>导入数据</span>
                    <input type="file" id="import-file" class="hidden" accept=".json">
                </button>
            </div>
        </aside>

        <!-- 移动端导航菜单按钮 -->
        <div class="md:hidden fixed bottom-4 right-4 z-50">
            <button id="mobile-menu-button" class="bg-primary text-white p-3 rounded-full shadow-lg">
                <i class="fa fa-bars"></i>
            </button>
        </div>

        <!-- 移动端导航菜单 -->
        <div id="mobile-menu" class="fixed inset-0 bg-dark bg-opacity-95 z-40 transform translate-x-full transition-transform duration-300 md:hidden">
            <div class="flex justify-end p-4">
                <button id="close-mobile-menu" class="text-white text-2xl">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <nav class="mt-5">
                <ul>
                    <li>
                        <a href="#dashboard" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 mobile-nav-link" data-target="dashboard">
                            <i class="fa fa-dashboard mr-3"></i>
                            <span>仪表盘</span>
                        </a>
                    </li>
                    <li>
                        <a href="#class-management" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 mobile-nav-link" data-target="class-management">
                            <i class="fa fa-users mr-3"></i>
                            <span>班级管理</span>
                        </a>
                    </li>
                    <li>
                        <a href="#student-management" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 mobile-nav-link" data-target="student-management">
                            <i class="fa fa-user mr-3"></i>
                            <span>学员管理</span>
                        </a>
                    </li>
                    <li>
                        <a href="#attendance" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 mobile-nav-link" data-target="attendance">
                            <i class="fa fa-check-square-o mr-3"></i>
                            <span>签到管理</span>
                        </a>
                    </li>
                    <li>
                        <a href="#statistics" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300 mobile-nav-link" data-target="statistics">
                            <i class="fa fa-bar-chart mr-3"></i>
                            <span>统计报表</span>
                        </a>
                    </li>
                    <li>
                        <a href="#" id="mobile-export-data" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300">
                            <i class="fa fa-download mr-3"></i>
                            <span>导出数据</span>
                        </a>
                    </li>
                    <li>
                        <a href="#" id="mobile-import-data" class="flex items-center px-4 py-3 text-gray-300 hover:bg-gray-700 hover:text-white transition-all-300">
                            <i class="fa fa-upload mr-3"></i>
                            <span>导入数据</span>
                            <input type="file" id="mobile-import-file" class="hidden" accept=".json">
                        </a>
                    </li>
                </ul>
            </nav>
        </div>

        <!-- 主内容区 -->
        <main class="flex-1 overflow-y-auto bg-gray-50">
            <!-- 顶部状态栏 -->
            <header class="bg-white shadow-sm">
                <div class="flex items-center justify-between px-6 py-4">
                    <div>
                        <h2 id="page-title" class="text-xl font-semibold text-gray-800">仪表盘</h2>
                        <p id="current-date" class="text-sm text-gray-600"></p>
                    </div>
                    <div class="flex items-center">
                        <div class="mr-4 text-right">
                            <p id="current-class-name" class="text-sm font-medium text-gray-800">未选择班级</p>
                            <p id="current-class-type" class="text-xs text-gray-500">请选择班级</p>
                        </div>
                        <select id="class-selector" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                            <option value="">选择班级</option>
                        </select>
                    </div>
                </div>
            </header>

            <!-- 页面内容区域 -->
            <div class="p-6">
                <!-- 仪表盘页面 -->
                <section id="dashboard" class="page-content active">
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
                        <!-- 今日签到统计卡片 -->
                        <div class="bg-white rounded-lg shadow p-6 transition-all-300 shadow-hover">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm font-medium text-gray-600">今日签到率</p>
                                    <p id="today-attendance-rate" class="text-2xl font-bold text-gray-800">0%</p>
                                </div>
                                <div class="p-3 bg-blue-100 rounded-full">
                                    <i class="fa fa-check-circle text-primary text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <div class="flex justify-between text-sm">
                                    <span>上午</span>
                                    <span id="morning-attendance">0/0</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="morning-attendance-bar" class="bg-primary rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="flex justify-between text-sm">
                                    <span>下午</span>
                                    <span id="afternoon-attendance">0/0</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="afternoon-attendance-bar" class="bg-primary rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                        </div>

                        <!-- 迟到统计卡片 -->
                        <div class="bg-white rounded-lg shadow p-6 transition-all-300 shadow-hover">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm font-medium text-gray-600">今日迟到</p>
                                    <p id="today-late-count" class="text-2xl font-bold text-gray-800">0</p>
                                </div>
                                <div class="p-3 bg-yellow-100 rounded-full">
                                    <i class="fa fa-clock-o text-warning text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <div class="flex justify-between text-sm">
                                    <span>上午</span>
                                    <span id="morning-late">0人</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="morning-late-bar" class="bg-warning rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="flex justify-between text-sm">
                                    <span>下午</span>
                                    <span id="afternoon-late">0人</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="afternoon-late-bar" class="bg-warning rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                        </div>

                        <!-- 旷课统计卡片 -->
                        <div class="bg-white rounded-lg shadow p-6 transition-all-300 shadow-hover">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm font-medium text-gray-600">今日旷课</p>
                                    <p id="today-absent-count" class="text-2xl font-bold text-gray-800">0</p>
                                </div>
                                <div class="p-3 bg-red-100 rounded-full">
                                    <i class="fa fa-times-circle text-danger text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <div class="flex justify-between text-sm">
                                    <span>上午</span>
                                    <span id="morning-absent">0人</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="morning-absent-bar" class="bg-danger rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="flex justify-between text-sm">
                                    <span>下午</span>
                                    <span id="afternoon-absent">0人</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="afternoon-absent-bar" class="bg-danger rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                        </div>

                        <!-- 请假统计卡片 -->
                        <div class="bg-white rounded-lg shadow p-6 transition-all-300 shadow-hover">
                            <div class="flex items-center justify-between">
                                <div>
                                    <p class="text-sm font-medium text-gray-600">今日请假</p>
                                    <p id="today-leave-count" class="text-2xl font-bold text-gray-800">0</p>
                                </div>
                                <div class="p-3 bg-gray-100 rounded-full">
                                    <i class="fa fa-calendar-minus-o text-secondary text-xl"></i>
                                </div>
                            </div>
                            <div class="mt-4">
                                <div class="flex justify-between text-sm">
                                    <span>上午</span>
                                    <span id="morning-leave">0人</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="morning-leave-bar" class="bg-secondary rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="mt-3">
                                <div class="flex justify-between text-sm">
                                    <span>下午</span>
                                    <span id="afternoon-leave">0人</span>
                                </div>
                                <div class="w-full bg-gray-200 rounded-full h-2 mt-1">
                                    <div id="afternoon-leave-bar" class="bg-secondary rounded-full h-2" style="width: 0%"></div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- 快速签到区域 -->
                    <div class="bg-white rounded-lg shadow p-6 mb-6">
                        <h3 class="text-lg font-semibold text-gray-800 mb-4">快速签到</h3>
                        <div class="flex flex-wrap gap-4">
                            <div class="flex items-center">
                                <label for="quick-date" class="mr-2 text-sm text-gray-600">日期:</label>
                                <input type="date" id="quick-date" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                            </div>
                            <div class="flex items-center">
                                <label for="quick-session" class="mr-2 text-sm text-gray-600">时段:</label>
                                <select id="quick-session" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="morning">上午</option>
                                    <option value="afternoon">下午</option>
                                </select>
                            </div>
                            <div class="flex items-center">
                                <label for="quick-status" class="mr-2 text-sm text-gray-600">状态:</label>
                                <select id="quick-status" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="present">全部签到</option>
                                    <option value="late">全部迟到</option>
                                    <option value="absent">全部旷课</option>
                                    <option value="leave">全部请假</option>
                                </select>
                            </div>
                            <button id="quick-attendance-btn" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                                一键签到
                            </button>
                        </div>
                    </div>

                    <!-- 最近签到记录 -->
                    <div class="bg-white rounded-lg shadow">
                        <div class="p-6 border-b border-gray-200">
                            <h3 class="text-lg font-semibold text-gray-800">最近签到记录</h3>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">日期</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">时段</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">签到人数</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">迟到人数</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">旷课人数</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">请假人数</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
                                    </tr>
                                </thead>
                                <tbody id="recent-records" class="bg-white divide-y divide-gray-200">
                                    <!-- 最近签到记录将通过JavaScript动态生成 -->
                                    <tr>
                                        <td colspan="7" class="px-6 py-4 text-center text-sm text-gray-500">暂无签到记录</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </section>

                <!-- 班级管理页面 -->
                <section id="class-management" class="page-content hidden">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-bold text-gray-800">班级管理</h2>
                        <button id="add-class-btn" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                            <i class="fa fa-plus mr-2"></i>添加班级
                        </button>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                        <!-- 班级卡片将通过JavaScript动态生成 -->
                        <div id="no-classes-message" class="col-span-full bg-white rounded-lg shadow p-6 text-center">
                            <i class="fa fa-info-circle text-4xl text-gray-400 mb-4"></i>
                            <h3 class="text-lg font-semibold text-gray-800 mb-2">暂无班级</h3>
                            <p class="text-gray-600 mb-4">点击"添加班级"按钮创建您的第一个班级</p>
                            <button id="add-first-class-btn" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                                <i class="fa fa-plus mr-2"></i>添加班级
                            </button>
                        </div>
                    </div>
                </section>

                <!-- 学员管理页面 -->
                <section id="student-management" class="page-content hidden">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-bold text-gray-800">学员管理</h2>
                        <div class="flex gap-3">
                            <button id="batch-import-btn" class="px-4 py-2 bg-secondary text-white rounded hover:bg-gray-600 transition-all-300">
                                <i class="fa fa-upload mr-2"></i>批量导入
                                <input type="file" id="batch-import-file" class="hidden" accept=".txt">
                            </button>
                            <button id="add-student-btn" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                                <i class="fa fa-plus mr-2"></i>添加学员
                            </button>
                        </div>
                    </div>

                    <div class="bg-white rounded-lg shadow overflow-hidden">
                        <div class="p-4 flex items-center justify-between border-b border-gray-200">
                            <div class="flex items-center">
                                <label for="student-search" class="mr-2 text-sm text-gray-600">搜索学员:</label>
                                <input type="text" id="student-search" placeholder="输入学员姓名搜索" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                            </div>
                            <div class="flex items-center">
                                <label for="student-class-filter" class="mr-2 text-sm text-gray-600">筛选班级:</label>
                                <select id="student-class-filter" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="">全部班级</option>
                                </select>
                            </div>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">序号</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">姓名</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">班级</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">出勤率</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">迟到次数</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">创建时间</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
                                    </tr>
                                </thead>
                                <tbody id="students-list" class="bg-white divide-y divide-gray-200">
                                    <!-- 学员列表将通过JavaScript动态生成 -->
                                    <tr>
                                        <td colspan="7" class="px-6 py-4 text-center text-sm text-gray-500">暂无学员数据</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <div class="px-6 py-4 border-t border-gray-200">
                            <div class="flex items-center justify-between">
                                <div class="text-sm text-gray-600">
                                    共 <span id="total-students">0</span> 名学员
                                </div>
                                <div class="flex space-x-2">
                                    <button id="prev-page" class="px-3 py-1 border border-gray-300 rounded-md text-sm disabled:opacity-50">上一页</button>
                                    <span id="current-page" class="px-3 py-1">1</span>
                                    <button id="next-page" class="px-3 py-1 border border-gray-300 rounded-md text-sm disabled:opacity-50">下一页</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- 签到管理页面 -->
                <section id="attendance" class="page-content hidden">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-bold text-gray-800">签到管理</h2>
                        <button id="refresh-attendance-btn" class="px-4 py-2 bg-secondary text-white rounded hover:bg-gray-600 transition-all-300">
                            <i class="fa fa-refresh mr-2"></i>刷新
                        </button>
                    </div>

                    <div class="bg-white rounded-lg shadow p-6 mb-6">
                        <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                            <div>
                                <label for="attendance-date" class="block text-sm font-medium text-gray-700 mb-1">日期</label>
                                <input type="date" id="attendance-date" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                            </div>
                            <div>
                                <label for="attendance-session" class="block text-sm font-medium text-gray-700 mb-1">时段</label>
                                <select id="attendance-session" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="morning">上午</option>
                                    <option value="afternoon">下午</option>
                                </select>
                            </div>
                            <div>
                                <label for="attendance-class" class="block text-sm font-medium text-gray-700 mb-1">班级</label>
                                <select id="attendance-class" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="">选择班级</option>
                                </select>
                            </div>
                            <div class="flex items-end">
                                <button id="load-attendance-btn" class="w-full px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                                    加载签到数据
                                </button>
                            </div>
                        </div>
                    </div>

                    <div id="attendance-list-container" class="bg-white rounded-lg shadow hidden">
                        <div class="p-4 flex items-center justify-between border-b border-gray-200">
                            <h3 class="text-lg font-semibold text-gray-800">
                                <span id="attendance-title">签到列表</span>
                                <span id="attendance-stats" class="ml-2 text-sm font-normal text-gray-600"></span>
                            </h3>
                            <div class="flex space-x-2">
                                <button id="batch-present-btn" class="px-3 py-1 bg-success text-white rounded hover:bg-green-600 transition-all-300 text-sm">
                                    全部签到
                                </button>
                                <button id="batch-absent-btn" class="px-3 py-1 bg-danger text-white rounded hover:bg-red-600 transition-all-300 text-sm">
                                    全部旷课
                                </button>
                                <button id="save-attendance-btn" class="px-3 py-1 bg-primary text-white rounded hover:bg-blue-600 transition-all-300 text-sm">
                                    保存签到
                                </button>
                            </div>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="min-w-full divide-y divide-gray-200">
                                <thead class="bg-gray-50">
                                    <tr>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">序号</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">姓名</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">状态</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">时间</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">备注</th>
                                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
                                    </tr>
                                </thead>
                                <tbody id="attendance-list" class="bg-white divide-y divide-gray-200">
                                    <!-- 签到列表将通过JavaScript动态生成 -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </section>

                <!-- 统计报表页面 -->
                <section id="statistics" class="page-content hidden">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-bold text-gray-800">统计报表</h2>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                        <!-- 班级考勤概览 -->
                        <div class="bg-white rounded-lg shadow p-6">
                            <h3 class="text-lg font-semibold text-gray-800 mb-4">班级考勤概览</h3>
                            <div class="h-64">
                                <canvas id="class-attendance-chart"></canvas>
                            </div>
                        </div>

                        <!-- 考勤状态分布 -->
                        <div class="bg-white rounded-lg shadow p-6">
                            <h3 class="text-lg font-semibold text-gray-800 mb-4">考勤状态分布</h3>
                            <div class="h-64">
                                <canvas id="attendance-status-chart"></canvas>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white rounded-lg shadow p-6 mb-6">
                        <h3 class="text-lg font-semibold text-gray-800 mb-4">学员考勤详情</h3>
                        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-4">
                            <div>
                                <label for="stats-class" class="block text-sm font-medium text-gray-700 mb-1">班级</label>
                                <select id="stats-class" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="">选择班级</option>
                                </select>
                            </div>
                            <div>
                                <label for="stats-student" class="block text-sm font-medium text-gray-700 mb-1">学员</label>
                                <select id="stats-student" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <option value="">选择学员</option>
                                </select>
                            </div>
                            <div>
                                <label for="stats-week" class="block text-sm font-medium text-gray-700 mb-1">周次</label>
                                <select id="stats-week" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <!-- 周次选项将通过JavaScript动态生成 -->
                                </select>
                            </div>
                            <div class="flex items-end">
                                <button id="load-stats-btn" class="w-full px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                                    加载统计
                                </button>
                            </div>
                        </div>
                        <div id="student-stats-container" class="hidden">
                            <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-4">
                                <div class="bg-blue-50 p-4 rounded-lg">
                                    <p class="text-sm text-gray-600">出勤率</p>
                                    <p id="student-attendance-rate" class="text-2xl font-bold text-primary">0%</p>
                                </div>
                                <div class="bg-yellow-50 p-4 rounded-lg">
                                    <p class="text-sm text-gray-600">迟到次数</p>
                                    <p id="student-late-count" class="text-2xl font-bold text-warning">0</p>
                                </div>
                                <div class="bg-red-50 p-4 rounded-lg">
                                    <p class="text-sm text-gray-600">旷课次数</p>
                                    <p id="student-absent-count" class="text-2xl font-bold text-danger">0</p>
                                </div>
                                <div class="bg-gray-50 p-4 rounded-lg">
                                    <p class="text-sm text-gray-600">请假次数</p>
                                    <p id="student-leave-count" class="text-2xl font-bold text-secondary">0</p>
                                </div>
                            </div>
                            <div class="h-64">
                                <canvas id="student-attendance-chart"></canvas>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white rounded-lg shadow p-6">
                        <div class="flex justify-between items-center mb-4">
                            <h3 class="text-lg font-semibold text-gray-800">周报表</h3>
                            <div class="flex items-center">
                                <label for="week-report-select" class="mr-2 text-sm text-gray-600">选择周次:</label>
                                <select id="week-report-select" class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary">
                                    <!-- 周次选项将通过JavaScript动态生成 -->
                                </select>
                                <button id="generate-week-report-btn" class="ml-2 px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                                    生成报表
                                </button>
                            </div>
                        </div>
                        <div id="week-report-container" class="hidden">
                            <div class="flex justify-between items-center mb-4">
                                <h4 id="week-report-title" class="text-lg font-semibold text-gray-800"></h4>
                                <button id="export-week-report-btn" class="px-3 py-1 bg-secondary text-white rounded hover:bg-gray-600 transition-all-300 text-sm">
                                    <i class="fa fa-download mr-1"></i>导出报表
                                </button>
                            </div>
                            <div class="overflow-x-auto">
                                <table class="min-w-full divide-y divide-gray-200">
                                    <thead class="bg-gray-50">
                                        <tr>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">学员姓名</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周一上午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周一下午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周二上午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周二下午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周三上午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周三下午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周四上午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周四下午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周五上午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">周五下午</th>
                                            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">出勤率</th>
                                        </tr>
                                    </thead>
                                    <tbody id="week-report-body" class="bg-white divide-y divide-gray-200">
                                        <!-- 周报表数据将通过JavaScript动态生成 -->
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </section>
            </div>
        </main>
    </div>

    <!-- 添加班级模态框 -->
    <div id="add-class-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg shadow-lg w-full max-w-md">
            <div class="p-4 border-b border-gray-200 flex justify-between items-center">
                <h3 class="text-lg font-semibold text-gray-800">添加班级</h3>
                <button class="close-modal text-gray-400 hover:text-gray-600">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <div class="p-6">
                <form id="add-class-form">
                    <div class="mb-4">
                        <label for="class-name" class="block text-sm font-medium text-gray-700 mb-1">班级名称</label>
                        <input type="text" id="class-name" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required>
                    </div>
                    <div class="mb-4">
                        <label for="class-type" class="block text-sm font-medium text-gray-700 mb-1">班级类型</label>
                        <select id="class-type" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required>
                            <option value="fulltime">全日制班级</option>
                            <option value="appointment">预约班级</option>
                        </select>
                    </div>
                    <div class="flex justify-end">
                        <button type="button" class="close-modal px-4 py-2 text-gray-700 hover:bg-gray-100 rounded mr-2">取消</button>
                        <button type="submit" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">保存</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- 添加学员模态框 -->
    <div id="add-student-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg shadow-lg w-full max-w-md">
            <div class="p-4 border-b border-gray-200 flex justify-between items-center">
                <h3 class="text-lg font-semibold text-gray-800">添加学员</h3>
                <button class="close-modal text-gray-400 hover:text-gray-600">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <div class="p-6">
                <form id="add-student-form">
                    <div class="mb-4">
                        <label for="student-name" class="block text-sm font-medium text-gray-700 mb-1">学员姓名</label>
                        <input type="text" id="student-name" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required>
                    </div>
                    <div class="mb-4">
                        <label for="student-class" class="block text-sm font-medium text-gray-700 mb-1">所属班级</label>
                        <select id="student-class" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required>
                            <option value="">选择班级</option>
                        </select>
                    </div>
                    <div class="flex justify-end">
                        <button type="button" class="close-modal px-4 py-2 text-gray-700 hover:bg-gray-100 rounded mr-2">取消</button>
                        <button type="submit" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">保存</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- 编辑学员模态框 -->
    <div id="edit-student-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg shadow-lg w-full max-w-md">
            <div class="p-4 border-b border-gray-200 flex justify-between items-center">
                <h3 class="text-lg font-semibold text-gray-800">编辑学员</h3>
                <button class="close-modal text-gray-400 hover:text-gray-600">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <div class="p-6">
                <form id="edit-student-form">
                    <input type="hidden" id="edit-student-id">
                    <div class="mb-4">
                        <label for="edit-student-name" class="block text-sm font-medium text-gray-700 mb-1">学员姓名</label>
                        <input type="text" id="edit-student-name" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required>
                    </div>
                    <div class="mb-4">
                        <label for="edit-student-class" class="block text-sm font-medium text-gray-700 mb-1">所属班级</label>
                        <select id="edit-student-class" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required>
                            <option value="">选择班级</option>
                        </select>
                    </div>
                    <div class="flex justify-end">
                        <button type="button" class="close-modal px-4 py-2 text-gray-700 hover:bg-gray-100 rounded mr-2">取消</button>
                        <button type="submit" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">保存</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- 学员考勤详情模态框 -->
    <div id="student-attendance-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg shadow-lg w-full max-w-4xl">
            <div class="p-4 border-b border-gray-200 flex justify-between items-center">
                <h3 id="student-attendance-title" class="text-lg font-semibold text-gray-800">学员考勤详情</h3>
                <button class="close-modal text-gray-400 hover:text-gray-600">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <div class="p-6">
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-4">
                    <div class="bg-blue-50 p-4 rounded-lg">
                        <p class="text-sm text-gray-600">总出勤次数</p>
                        <p id="detail-attendance-count" class="text-2xl font-bold text-primary">0</p>
                    </div>
                    <div class="bg-yellow-50 p-4 rounded-lg">
                        <p class="text-sm text-gray-600">总迟到次数</p>
                        <p id="detail-late-count" class="text-2xl font-bold text-warning">0</p>
                    </div>
                    <div class="bg-red-50 p-4 rounded-lg">
                        <p class="text-sm text-gray-600">总旷课次数</p>
                        <p id="detail-absent-count" class="text-2xl font-bold text-danger">0</p>
                    </div>
                    <div class="bg-gray-50 p-4 rounded-lg">
                        <p class="text-sm text-gray-600">总请假次数</p>
                        <p id="detail-leave-count" class="text-2xl font-bold text-secondary">0</p>
                    </div>
                </div>
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead class="bg-gray-50">
                            <tr>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">日期</th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">时段</th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">状态</th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">时间</th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">备注</th>
                            </tr>
                        </thead>
                        <tbody id="student-attendance-details" class="bg-white divide-y divide-gray-200">
                            <!-- 学员考勤详情将通过JavaScript动态生成 -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- 请假备注模态框 -->
    <div id="leave-remark-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
        <div class="bg-white rounded-lg shadow-lg w-full max-w-md">
            <div class="p-4 border-b border-gray-200 flex justify-between items-center">
                <h3 class="text-lg font-semibold text-gray-800">请假备注</h3>
                <button class="close-modal text-gray-400 hover:text-gray-600">
                    <i class="fa fa-times"></i>
                </button>
            </div>
            <div class="p-6">
                <form id="leave-remark-form">
                    <input type="hidden" id="leave-student-id">
                    <input type="hidden" id="leave-record-id">
                    <div class="mb-4">
                        <label for="leave-reason" class="block text-sm font-medium text-gray-700 mb-1">请假原因</label>
                        <textarea id="leave-reason" rows="4" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-primary" required></textarea>
                    </div>
                    <div class="flex justify-end">
                        <button type="button" class="close-modal px-4 py-2 text-gray-700 hover:bg-gray-100 rounded mr-2">取消</button>
                        <button type="submit" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">保存</button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- 提示消息 -->
    <div id="toast" class="fixed bottom-4 right-4 bg-white rounded-lg shadow-lg p-4 transform translate-y-10 opacity-0 transition-all duration-300 z-50 max-w-sm">
        <div class="flex items-center">
            <div id="toast-icon" class="mr-3 text-xl"></div>
            <div>
                <h4 id="toast-title" class="font-semibold text-gray-800"></h4>
                <p id="toast-message" class="text-sm text-gray-600"></p>
            </div>
        </div>
    </div>

    <script>
        // 全局变量
        let classes = [];
        let students = [];
        let attendanceRecords = [];
        let currentClass = null;
        let currentPage = 1;
        const studentsPerPage = 10;
        let filteredStudents = [];
        let currentAttendanceDate = '';
        let currentAttendanceSession = 'morning';
        let currentAttendanceClass = '';
        let currentAttendanceRecords = [];
        let currentStudentStats = null;
        let currentWeekReport = null;

        // DOM 加载完成后执行
        document.addEventListener('DOMContentLoaded', function() {
            // 初始化数据
            initData();
            
            // 设置当前日期
            const today = new Date();
            const dateString = today.toISOString().split('T')[0];
            document.getElementById('quick-date').value = dateString;
            document.getElementById('attendance-date').value = dateString;
            document.getElementById('current-date').textContent = formatDate(today);
            
            // 初始化页面
            updateClassSelector();
            updateDashboard();
            loadRecentRecords();
            
            // 事件监听
            setupEventListeners();
        });

        // 初始化数据
        function initData() {
            // 从 localStorage 加载数据
            const savedClasses = localStorage.getItem('attendanceClasses');
            const savedStudents = localStorage.getItem('attendanceStudents');
            const savedRecords = localStorage.getItem('attendanceRecords');
            
            if (savedClasses) {
                classes = JSON.parse(savedClasses);
            }
            
            if (savedStudents) {
                students = JSON.parse(savedStudents);
            }
            
            if (savedRecords) {
                attendanceRecords = JSON.parse(savedRecords);
            }
            
            // 如果没有数据，创建示例数据
            if (classes.length === 0 && students.length === 0) {
                createSampleData();
            }
        }

        // 创建示例数据
        function createSampleData() {
            // 创建示例班级
            const fulltimeClass = {
                id: 'class-' + Date.now(),
                name: '全日制班级A',
                type: 'fulltime',
                students: [],
                createTime: new Date().toISOString()
            };
            
            const appointmentClass = {
                id: 'class-' + (Date.now() + 1),
                name: '预约班级B',
                type: 'appointment',
                students: [],
                createTime: new Date().toISOString()
            };
            
            classes.push(fulltimeClass);
            classes.push(appointmentClass);
            
            // 创建示例学员
            const sampleStudents = [
                { name: '张三', classId: fulltimeClass.id },
                { name: '李四', classId: fulltimeClass.id },
                { name: '王五', classId: fulltimeClass.id },
                { name: '赵六', classId: fulltimeClass.id },
                { name: '钱七', classId: fulltimeClass.id },
                { name: '孙八', classId: appointmentClass.id },
                { name: '周九', classId: appointmentClass.id },
                { name: '吴十', classId: appointmentClass.id }
            ];
            
            sampleStudents.forEach((student, index) => {
                const newStudent = {
                    id: 'student-' + (Date.now() + index),
                    name: student.name,
                    classId: student.classId,
                    createTime: new Date().toISOString()
                };
                
                students.push(newStudent);
                
                // 将学员添加到班级
                const classIndex = classes.findIndex(c => c.id === student.classId);
                if (classIndex !== -1) {
                    classes[classIndex].students.push(newStudent.id);
                }
            });
            
            // 创建过去7天的示例签到记录
            const today = new Date();
            for (let i = 0; i < 7; i++) {
                const date = new Date(today);
                date.setDate(today.getDate() - i);
                const dateString = date.toISOString().split('T')[0];
                
                // 上午签到记录
                createSampleAttendanceRecord(dateString, 'morning', fulltimeClass.id);
                createSampleAttendanceRecord(dateString, 'morning', appointmentClass.id);
                
                // 下午签到记录
                createSampleAttendanceRecord(dateString, 'afternoon', fulltimeClass.id);
                createSampleAttendanceRecord(dateString, 'afternoon', appointmentClass.id);
            }
            
            // 保存数据
            saveData();
        }

        // 创建示例签到记录
        function createSampleAttendanceRecord(date, session, classId) {
            const classObj = classes.find(c => c.id === classId);
            if (!classObj) return;
            
            const record = {
                id: 'record-' + Date.now() + Math.random().toString(36).substr(2, 9),
                date: date,
                session: session,
                classId: classId,
                records: []
            };
            
            // 获取班级学员
            const classStudents = students.filter(s => s.classId === classId);
            
            // 为每个学员创建签到记录
            classStudents.forEach(student => {
                // 随机生成签到状态
                const statusOptions = ['present', 'late', 'absent', 'leave'];
                const weights = [0.7, 0.15, 0.1, 0.05]; // 权重：签到70%，迟到15%，旷课10%，请假5%
                const status = weightedRandom(statusOptions, weights);
                
                const studentRecord = {
                    studentId: student.id,
                    status: status,
                    time: getRandomTime(session),
                    lateMinutes: status === 'late' ? Math.floor(Math.random() * 30) + 1 : 0,
                    remark: status === 'leave' ? getRandomLeaveReason() : ''
                };
                
                record.records.push(studentRecord);
            });
            
            attendanceRecords.push(record);
        }

        // 加权随机选择
        function weightedRandom(options, weights) {
            const totalWeight = weights.reduce((sum, weight) => sum + weight, 0);
            let random = Math.random() * totalWeight;
            
            for (let i = 0; i < options.length; i++) {
                random -= weights[i];
                if (random <= 0) {
                    return options[i];
                }
            }
            
            return options[options.length - 1];
        }

        // 获取随机时间
        function getRandomTime(session) {
            let hour, minute;
            
            if (session === 'morning') {
                hour = Math.floor(Math.random() * 2) + 8; // 8-9点
                minute = Math.floor(Math.random() * 60);
            } else {
                hour = Math.floor(Math.random() * 2) + 14; // 14-15点
                minute = Math.floor(Math.random() * 60);
            }
            
            return `${hour.toString().padStart(2, '0')}:${minute.toString().padStart(2, '0')}`;
        }

        // 获取随机请假原因
        function getRandomLeaveReason() {
            const reasons = [
                '身体不适',
                '家中有事',
                '个人原因',
                '临时有事',
                '生病请假'
            ];
            
            return reasons[Math.floor(Math.random() * reasons.length)];
        }

        // 保存数据到 localStorage
        function saveData() {
            localStorage.setItem('attendanceClasses', JSON.stringify(classes));
            localStorage.setItem('attendanceStudents', JSON.stringify(students));
            localStorage.setItem('attendanceRecords', JSON.stringify(attendanceRecords));
        }

        // 设置事件监听
        function setupEventListeners() {
            // 导航链接点击事件
            document.querySelectorAll('.nav-link').forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    const target = this.getAttribute('data-target');
                    showPage(target);
                });
            });
            
            // 移动端导航菜单
            document.getElementById('mobile-menu-button').addEventListener('click', function() {
                document.getElementById('mobile-menu').classList.remove('translate-x-full');
            });
            
            document.getElementById('close-mobile-menu').addEventListener('click', function() {
                document.getElementById('mobile-menu').classList.add('translate-x-full');
            });
            
            document.querySelectorAll('.mobile-nav-link').forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    const target = this.getAttribute('data-target');
                    showPage(target);
                    document.getElementById('mobile-menu').classList.add('translate-x-full');
                });
            });
            
            // 班级选择器变更事件
            document.getElementById('class-selector').addEventListener('change', function() {
                const classId = this.value;
                if (classId) {
                    currentClass = classes.find(c => c.id === classId);
                    document.getElementById('current-class-name').textContent = currentClass.name;
                    document.getElementById('current-class-type').textContent = currentClass.type === 'fulltime' ? '全日制班级' : '预约班级';
                } else {
                    currentClass = null;
                    document.getElementById('current-class-name').textContent = '未选择班级';
                    document.getElementById('current-class-type').textContent = '请选择班级';
                }
                
                updateDashboard();
            });
            
            // 添加班级按钮点击事件
            document.getElementById('add-class-btn').addEventListener('click', function() {
                document.getElementById('add-class-modal').classList.remove('hidden');
            });
            
            document.getElementById('add-first-class-btn').addEventListener('click', function() {
                document.getElementById('add-class-modal').classList.remove('hidden');
            });
            
            // 添加班级表单提交事件
            document.getElementById('add-class-form').addEventListener('submit', function(e) {
                e.preventDefault();
                addClass();
            });
            
            // 添加学员按钮点击事件
            document.getElementById('add-student-btn').addEventListener('click', function() {
                updateStudentClassSelector();
                document.getElementById('add-student-modal').classList.remove('hidden');
            });
            
            // 添加学员表单提交事件
            document.getElementById('add-student-form').addEventListener('submit', function(e) {
                e.preventDefault();
                addStudent();
            });
            
            // 批量导入按钮点击事件
            document.getElementById('batch-import-btn').addEventListener('click', function() {
                document.getElementById('batch-import-file').click();
            });
            
            // 批量导入文件变更事件
            document.getElementById('batch-import-file').addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    batchImportStudents(file);
                }
            });
            
            // 学员搜索输入事件
            document.getElementById('student-search').addEventListener('input', function() {
                filterStudents();
            });
            
            // 学员班级筛选变更事件
            document.getElementById('student-class-filter').addEventListener('change', function() {
                filterStudents();
            });
            
            // 学员分页按钮点击事件
            document.getElementById('prev-page').addEventListener('click', function() {
                if (currentPage > 1) {
                    currentPage--;
                    renderStudentsList();
                }
            });
            
            document.getElementById('next-page').addEventListener('click', function() {
                const maxPage = Math.ceil(filteredStudents.length / studentsPerPage);
                if (currentPage < maxPage) {
                    currentPage++;
                    renderStudentsList();
                }
            });
            
            // 快速签到按钮点击事件
            document.getElementById('quick-attendance-btn').addEventListener('click', function() {
                quickAttendance();
            });
            
            // 加载签到数据按钮点击事件
            document.getElementById('load-attendance-btn').addEventListener('click', function() {
                loadAttendanceData();
            });
            
            // 批量签到按钮点击事件
            document.getElementById('batch-present-btn').addEventListener('click', function() {
                batchUpdateAttendance('present');
            });
            
            // 批量旷课按钮点击事件
            document.getElementById('batch-absent-btn').addEventListener('click', function() {
                batchUpdateAttendance('absent');
            });
            
            // 保存签到按钮点击事件
            document.getElementById('save-attendance-btn').addEventListener('click', function() {
                saveAttendanceData();
            });
            
            // 刷新签到按钮点击事件
            document.getElementById('refresh-attendance-btn').addEventListener('click', function() {
                document.getElementById('attendance-list-container').classList.add('hidden');
                document.getElementById('attendance-date').value = new Date().toISOString().split('T')[0];
                document.getElementById('attendance-session').value = 'morning';
                document.getElementById('attendance-class').value = '';
                currentAttendanceRecords = [];
            });
            
            // 请假备注表单提交事件
            document.getElementById('leave-remark-form').addEventListener('submit', function(e) {
                e.preventDefault();
                saveLeaveRemark();
            });
            
            // 加载统计按钮点击事件
            document.getElementById('load-stats-btn').addEventListener('click', function() {
                loadStudentStats();
            });
            
            // 生成周报表按钮点击事件
            document.getElementById('generate-week-report-btn').addEventListener('click', function() {
                generateWeekReport();
            });
            
            // 导出周报表按钮点击事件
            document.getElementById('export-week-report-btn').addEventListener('click', function() {
                exportWeekReport();
            });
            
            // 导出数据按钮点击事件
            document.getElementById('export-data').addEventListener('click', function() {
                exportData();
            });
            
            document.getElementById('mobile-export-data').addEventListener('click', function(e) {
                e.preventDefault();
                exportData();
                document.getElementById('mobile-menu').classList.add('translate-x-full');
            });
            
            // 导入数据按钮点击事件
            document.getElementById('import-data').addEventListener('click', function() {
                document.getElementById('import-file').click();
            });
            
            document.getElementById('mobile-import-data').addEventListener('click', function(e) {
                e.preventDefault();
                document.getElementById('mobile-import-file').click();
                document.getElementById('mobile-menu').classList.add('translate-x-full');
            });
            
            // 导入数据文件变更事件
            document.getElementById('import-file').addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    importData(file);
                }
            });
            
            document.getElementById('mobile-import-file').addEventListener('change', function(e) {
                const file = e.target.files[0];
                if (file) {
                    importData(file);
                }
            });
            
            // 关闭模态框按钮点击事件
            document.querySelectorAll('.close-modal').forEach(button => {
                button.addEventListener('click', function() {
                    this.closest('.fixed').classList.add('hidden');
                });
            });
            
            // 点击模态框背景关闭模态框
            document.querySelectorAll('.fixed').forEach(modal => {
                modal.addEventListener('click', function(e) {
                    if (e.target === this) {
                        this.classList.add('hidden');
                    }
                });
            });
        }

        // 显示页面
        function showPage(pageId) {
            // 隐藏所有页面
            document.querySelectorAll('.page-content').forEach(page => {
                page.classList.add('hidden');
            });
            
            // 显示目标页面
            document.getElementById(pageId).classList.remove('hidden');
            
            // 更新页面标题
            const pageTitle = document.getElementById('page-title');
            switch (pageId) {
                case 'dashboard':
                    pageTitle.textContent = '仪表盘';
                    updateDashboard();
                    break;
                case 'class-management':
                    pageTitle.textContent = '班级管理';
                    renderClassesList();
                    break;
                case 'student-management':
                    pageTitle.textContent = '学员管理';
                    updateStudentClassSelector();
                    updateStudentFilterSelector();
                    filterStudents();
                    break;
                case 'attendance':
                    pageTitle.textContent = '签到管理';
                    updateAttendanceClassSelector();
                    break;
                case 'statistics':
                    pageTitle.textContent = '统计报表';
                    updateStatsClassSelector();
                    updateWeekSelector();
                    renderCharts();
                    break;
            }
            
            // 更新导航链接状态
            document.querySelectorAll('.nav-link').forEach(link => {
                if (link.getAttribute('data-target') === pageId) {
                    link.classList.add('active', 'bg-gray-700', 'text-white');
                    link.classList.remove('text-gray-300');
                } else {
                    link.classList.remove('active', 'bg-gray-700', 'text-white');
                    link.classList.add('text-gray-300');
                }
            });
        }

        // 更新班级选择器
        function updateClassSelector() {
            const selector = document.getElementById('class-selector');
            selector.innerHTML = '<option value="">选择班级</option>';
            
            classes.forEach(classObj => {
                const option = document.createElement('option');
                option.value = classObj.id;
                option.textContent = classObj.name;
                selector.appendChild(option);
            });
            
            // 如果有当前班级，选中它
            if (currentClass) {
                selector.value = currentClass.id;
            }
        }

        // 更新签到页面班级选择器
        function updateAttendanceClassSelector() {
            const selector = document.getElementById('attendance-class');
            selector.innerHTML = '<option value="">选择班级</option>';
            
            classes.forEach(classObj => {
                const option = document.createElement('option');
                option.value = classObj.id;
                option.textContent = classObj.name;
                selector.appendChild(option);
            });
        }

        // 更新统计页面班级选择器
        function updateStatsClassSelector() {
            const classSelector = document.getElementById('stats-class');
            classSelector.innerHTML = '<option value="">选择班级</option>';
            
            classes.forEach(classObj => {
                const option = document.createElement('option');
                option.value = classObj.id;
                option.textContent = classObj.name;
                classSelector.appendChild(option);
            });
            
            // 更新学员选择器（初始为空）
            const studentSelector = document.getElementById('stats-student');
            studentSelector.innerHTML = '<option value="">选择学员</option>';
        }

        // 更新统计页面周次选择器
        function updateWeekSelector() {
            const weekSelector = document.getElementById('stats-week');
            const reportWeekSelector = document.getElementById('week-report-select');
            
            // 清空现有选项
            weekSelector.innerHTML = '';
            reportWeekSelector.innerHTML = '';
            
            // 获取最早的签到记录日期
            let earliestDate = new Date();
            if (attendanceRecords.length > 0) {
                earliestDate = new Date(Math.min(...attendanceRecords.map(r => new Date(r.date))));
            }
            
            // 获取当前日期
            const today = new Date();
            
            // 计算从最早日期到当前日期的周数
            const weeks = [];
            const currentWeekStart = getWeekStart(today);
            const earliestWeekStart = getWeekStart(earliestDate);
            
            let weekStart = currentWeekStart;
            while (weekStart >= earliestWeekStart || weeks.length < 12) { // 至少显示12周
                const weekEnd = new Date(weekStart);
                weekEnd.setDate(weekStart.getDate() + 6);
                
                const weekLabel = `${formatDate(weekStart)} 至 ${formatDate(weekEnd)}`;
                const weekValue = weekStart.toISOString().split('T')[0];
                
                weeks.push({
                    label: weekLabel,
                    value: weekValue
                });
                
                // 上一周
                weekStart = new Date(weekStart);
                weekStart.setDate(weekStart.getDate() - 7);
            }
            
            // 添加选项
            weeks.forEach(week => {
                const option1 = document.createElement('option');
                option1.value = week.value;
                option1.textContent = week.label;
                weekSelector.appendChild(option1);
                
                const option2 = document.createElement('option');
                option2.value = week.value;
                option2.textContent = week.label;
                reportWeekSelector.appendChild(option2);
            });
            
            // 默认选中当前周
            weekSelector.value = currentWeekStart.toISOString().split('T')[0];
            reportWeekSelector.value = currentWeekStart.toISOString().split('T')[0];
        }

        // 获取一周的开始日期（周一）
        function getWeekStart(date) {
            const d = new Date(date);
            const day = d.getDay();
            const diff = d.getDate() - day + (day === 0 ? -6 : 1); // 调整为周一为一周的开始
            return new Date(d.setDate(diff));
        }

        // 更新学员管理页面班级选择器
        function updateStudentClassSelector() {
            const selector = document.getElementById('student-class');
            selector.innerHTML = '<option value="">选择班级</option>';
            
            classes.forEach(classObj => {
                const option = document.createElement('option');
                option.value = classObj.id;
                option.textContent = classObj.name;
                selector.appendChild(option);
            });
        }

        // 更新编辑学员页面班级选择器
        function updateEditStudentClassSelector() {
            const selector = document.getElementById('edit-student-class');
            selector.innerHTML = '<option value="">选择班级</option>';
            
            classes.forEach(classObj => {
                const option = document.createElement('option');
                option.value = classObj.id;
                option.textContent = classObj.name;
                selector.appendChild(option);
            });
        }

        // 更新学员筛选班级选择器
        function updateStudentFilterSelector() {
            const selector = document.getElementById('student-class-filter');
            selector.innerHTML = '<option value="">全部班级</option>';
            
            classes.forEach(classObj => {
                const option = document.createElement('option');
                option.value = classObj.id;
                option.textContent = classObj.name;
                selector.appendChild(option);
            });
        }

        // 更新仪表盘
        function updateDashboard() {
            if (!currentClass) {
                // 如果没有选择班级，显示所有班级的汇总数据
                updateDashboardSummary();
            } else {
                // 显示当前班级的数据
                updateDashboardForClass(currentClass.id);
            }
            
            loadRecentRecords();
        }

        // 更新仪表盘汇总数据
        function updateDashboardSummary() {
            const today = new Date().toISOString().split('T')[0];
            
            // 获取今日上午和下午的签到记录
            const morningRecords = attendanceRecords.find(r => r.date === today && r.session === 'morning');
            const afternoonRecords = attendanceRecords.find(r => r.date === today && r.session === 'afternoon');
            
            // 计算上午签到统计
            let morningTotal = 0;
            let morningPresent = 0;
            let morningLate = 0;
            let morningAbsent = 0;
            let morningLeave = 0;
            
            if (morningRecords) {
                morningTotal = morningRecords.records.length;
                morningPresent = morningRecords.records.filter(r => r.status === 'present').length;
                morningLate = morningRecords.records.filter(r => r.status === 'late').length;
                morningAbsent = morningRecords.records.filter(r => r.status === 'absent').length;
                morningLeave = morningRecords.records.filter(r => r.status === 'leave').length;
            }
            
            // 计算下午签到统计
            let afternoonTotal = 0;
            let afternoonPresent = 0;
            let afternoonLate = 0;
            let afternoonAbsent = 0;
            let afternoonLeave = 0;
            
            if (afternoonRecords) {
                afternoonTotal = afternoonRecords.records.length;
                afternoonPresent = afternoonRecords.records.filter(r => r.status === 'present').length;
                afternoonLate = afternoonRecords.records.filter(r => r.status === 'late').length;
                afternoonAbsent = afternoonRecords.records.filter(r => r.status === 'absent').length;
                afternoonLeave = afternoonRecords.records.filter(r => r.status === 'leave').length;
            }
            
            // 计算总签到率
            const totalStudents = students.length;
            const totalSessions = 2; // 上午和下午
            const totalPossibleAttendances = totalStudents * totalSessions;
            const totalActualAttendances = morningPresent + morningLate + afternoonPresent + afternoonLate;
            const attendanceRate = totalPossibleAttendances > 0 ? Math.round((totalActualAttendances / totalPossibleAttendances) * 100) : 0;
            
            // 更新UI
            document.getElementById('today-attendance-rate').textContent = `${attendanceRate}%`;
            
            document.getElementById('morning-attendance').textContent = `${morningPresent + morningLate}/${morningTotal}`;
            document.getElementById('morning-attendance-bar').style.width = morningTotal > 0 ? `${((morningPresent + morningLate) / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-attendance').textContent = `${afternoonPresent + afternoonLate}/${afternoonTotal}`;
            document.getElementById('afternoon-attendance-bar').style.width = afternoonTotal > 0 ? `${((afternoonPresent + afternoonLate) / afternoonTotal) * 100}%` : '0%';
            
            document.getElementById('today-late-count').textContent = morningLate + afternoonLate;
            document.getElementById('morning-late').textContent = `${morningLate}人`;
            document.getElementById('morning-late-bar').style.width = morningTotal > 0 ? `${(morningLate / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-late').textContent = `${afternoonLate}人`;
            document.getElementById('afternoon-late-bar').style.width = afternoonTotal > 0 ? `${(afternoonLate / afternoonTotal) * 100}%` : '0%';
            
            document.getElementById('today-absent-count').textContent = morningAbsent + afternoonAbsent;
            document.getElementById('morning-absent').textContent = `${morningAbsent}人`;
            document.getElementById('morning-absent-bar').style.width = morningTotal > 0 ? `${(morningAbsent / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-absent').textContent = `${afternoonAbsent}人`;
            document.getElementById('afternoon-absent-bar').style.width = afternoonTotal > 0 ? `${(afternoonAbsent / afternoonTotal) * 100}%` : '0%';
            
            document.getElementById('today-leave-count').textContent = morningLeave + afternoonLeave;
            document.getElementById('morning-leave').textContent = `${morningLeave}人`;
            document.getElementById('morning-leave-bar').style.width = morningTotal > 0 ? `${(morningLeave / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-leave').textContent = `${afternoonLeave}人`;
            document.getElementById('afternoon-leave-bar').style.width = afternoonTotal > 0 ? `${(afternoonLeave / afternoonTotal) * 100}%` : '0%';
        }

        // 更新指定班级的仪表盘数据
        function updateDashboardForClass(classId) {
            const today = new Date().toISOString().split('T')[0];
            
            // 获取今日上午和下午的签到记录
            const morningRecords = attendanceRecords.find(r => r.date === today && r.session === 'morning' && r.classId === classId);
            const afternoonRecords = attendanceRecords.find(r => r.date === today && r.session === 'afternoon' && r.classId === classId);
            
            // 获取班级学员数量
            const classStudents = students.filter(s => s.classId === classId);
            const totalStudents = classStudents.length;
            
            // 计算上午签到统计
            let morningTotal = 0;
            let morningPresent = 0;
            let morningLate = 0;
            let morningAbsent = 0;
            let morningLeave = 0;
            
            if (morningRecords) {
                morningTotal = morningRecords.records.length;
                morningPresent = morningRecords.records.filter(r => r.status === 'present').length;
                morningLate = morningRecords.records.filter(r => r.status === 'late').length;
                morningAbsent = morningRecords.records.filter(r => r.status === 'absent').length;
                morningLeave = morningRecords.records.filter(r => r.status === 'leave').length;
            }
            
            // 计算下午签到统计
            let afternoonTotal = 0;
            let afternoonPresent = 0;
            let afternoonLate = 0;
            let afternoonAbsent = 0;
            let afternoonLeave = 0;
            
            if (afternoonRecords) {
                afternoonTotal = afternoonRecords.records.length;
                afternoonPresent = afternoonRecords.records.filter(r => r.status === 'present').length;
                afternoonLate = afternoonRecords.records.filter(r => r.status === 'late').length;
                afternoonAbsent = afternoonRecords.records.filter(r => r.status === 'absent').length;
                afternoonLeave = afternoonRecords.records.filter(r => r.status === 'leave').length;
            }
            
            // 计算总签到率
            const totalSessions = 2; // 上午和下午
            const totalPossibleAttendances = totalStudents * totalSessions;
            const totalActualAttendances = morningPresent + morningLate + afternoonPresent + afternoonLate;
            const attendanceRate = totalPossibleAttendances > 0 ? Math.round((totalActualAttendances / totalPossibleAttendances) * 100) : 0;
            
            // 更新UI
            document.getElementById('today-attendance-rate').textContent = `${attendanceRate}%`;
            
            document.getElementById('morning-attendance').textContent = `${morningPresent + morningLate}/${morningTotal}`;
            document.getElementById('morning-attendance-bar').style.width = morningTotal > 0 ? `${((morningPresent + morningLate) / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-attendance').textContent = `${afternoonPresent + afternoonLate}/${afternoonTotal}`;
            document.getElementById('afternoon-attendance-bar').style.width = afternoonTotal > 0 ? `${((afternoonPresent + afternoonLate) / afternoonTotal) * 100}%` : '0%';
            
            document.getElementById('today-late-count').textContent = morningLate + afternoonLate;
            document.getElementById('morning-late').textContent = `${morningLate}人`;
            document.getElementById('morning-late-bar').style.width = morningTotal > 0 ? `${(morningLate / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-late').textContent = `${afternoonLate}人`;
            document.getElementById('afternoon-late-bar').style.width = afternoonTotal > 0 ? `${(afternoonLate / afternoonTotal) * 100}%` : '0%';
            
            document.getElementById('today-absent-count').textContent = morningAbsent + afternoonAbsent;
            document.getElementById('morning-absent').textContent = `${morningAbsent}人`;
            document.getElementById('morning-absent-bar').style.width = morningTotal > 0 ? `${(morningAbsent / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-absent').textContent = `${afternoonAbsent}人`;
            document.getElementById('afternoon-absent-bar').style.width = afternoonTotal > 0 ? `${(afternoonAbsent / afternoonTotal) * 100}%` : '0%';
            
            document.getElementById('today-leave-count').textContent = morningLeave + afternoonLeave;
            document.getElementById('morning-leave').textContent = `${morningLeave}人`;
            document.getElementById('morning-leave-bar').style.width = morningTotal > 0 ? `${(morningLeave / morningTotal) * 100}%` : '0%';
            
            document.getElementById('afternoon-leave').textContent = `${afternoonLeave}人`;
            document.getElementById('afternoon-leave-bar').style.width = afternoonTotal > 0 ? `${(afternoonLeave / afternoonTotal) * 100}%` : '0%';
        }

        // 加载最近签到记录
        function loadRecentRecords() {
            const recentRecordsContainer = document.getElementById('recent-records');
            recentRecordsContainer.innerHTML = '';
            
            // 获取最近7天的日期
            const dates = [];
            const today = new Date();
            for (let i = 0; i < 7; i++) {
                const date = new Date(today);
                date.setDate(today.getDate() - i);
                dates.push(date.toISOString().split('T')[0]);
            }
            
            // 筛选最近7天的签到记录
            let filteredRecords = attendanceRecords.filter(r => dates.includes(r.date));
            
            // 如果有当前班级，只显示当前班级的记录
            if (currentClass) {
                filteredRecords = filteredRecords.filter(r => r.classId === currentClass.id);
            }
            
            // 按日期和时段排序
            filteredRecords.sort((a, b) => {
                if (a.date !== b.date) {
                    return new Date(b.date) - new Date(a.date);
                }
                return a.session === 'morning' ? -1 : 1;
            });
            
            // 显示最近10条记录
            const recentRecords = filteredRecords.slice(0, 10);
            
            if (recentRecords.length === 0) {
                const row = document.createElement('tr');
                row.innerHTML = `<td colspan="7" class="px-6 py-4 text-center text-sm text-gray-500">暂无签到记录</td>`;
                recentRecordsContainer.appendChild(row);
                return;
            }
            
            recentRecords.forEach(record => {
                const classObj = classes.find(c => c.id === record.classId);
                const className = classObj ? classObj.name : '未知班级';
                
                const presentCount = record.records.filter(r => r.status === 'present').length;
                const lateCount = record.records.filter(r => r.status === 'late').length;
                const absentCount = record.records.filter(r => r.status === 'absent').length;
                const leaveCount = record.records.filter(r => r.status === 'leave').length;
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${formatDate(new Date(record.date))}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${record.session === 'morning' ? '上午' : '下午'}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${presentCount}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${lateCount}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${absentCount}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${leaveCount}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">
                        <span class="inline-block px-2 py-1 text-xs font-semibold text-gray-800 bg-gray-200 rounded">${className}</span>
                    </td>
                `;
                
                recentRecordsContainer.appendChild(row);
            });
        }

        // 渲染班级列表
        function renderClassesList() {
            const container = document.querySelector('#class-management .grid');
            container.innerHTML = '';
            
            if (classes.length === 0) {
                container.innerHTML = `
                    <div id="no-classes-message" class="col-span-full bg-white rounded-lg shadow p-6 text-center">
                        <i class="fa fa-info-circle text-4xl text-gray-400 mb-4"></i>
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">暂无班级</h3>
                        <p class="text-gray-600 mb-4">点击"添加班级"按钮创建您的第一个班级</p>
                        <button id="add-first-class-btn" class="px-4 py-2 bg-primary text-white rounded hover:bg-blue-600 transition-all-300">
                            <i class="fa fa-plus mr-2"></i>添加班级
                        </button>
                    </div>
                `;
                
                // 重新绑定事件
                document.getElementById('add-first-class-btn').addEventListener('click', function() {
                    document.getElementById('add-class-modal').classList.remove('hidden');
                });
                
                return;
            }
            
            classes.forEach(classObj => {
                const classStudents = students.filter(s => s.classId === classObj.id);
                const classType = classObj.type === 'fulltime' ? '全日制班级' : '预约班级';
                
                const card = document.createElement('div');
                card.className = 'bg-white rounded-lg shadow overflow-hidden transition-all-300 shadow-hover';
                card.innerHTML = `
                    <div class="p-6">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="text-lg font-semibold text-gray-800">${classObj.name}</h3>
                                <p class="text-sm text-gray-600 mt-1">${classType}</p>
                            </div>
                            <div class="flex space-x-2">
                                <button class="edit-class-btn p-2 text-gray-500 hover:text-primary" data-class-id="${classObj.id}">
                                    <i class="fa fa-pencil"></i>
                                </button>
                                <button class="delete-class-btn p-2 text-gray-500 hover:text-danger" data-class-id="${classObj.id}">
                                    <i class="fa fa-trash"></i>
                                </button>
                            </div>
                        </div>
                        <div class="mt-4">
                            <div class="flex justify-between text-sm">
                                <span class="text-gray-600">学员数量</span>
                                <span class="font-medium">${classStudents.length} 人</span>
                            </div>
                            <div class="flex justify-between text-sm mt-2">
                                <span class="text-gray-600">创建时间</span>
                                <span>${formatDate(new Date(classObj.createTime))}</span>
                            </div>
                        </div>
                    </div>
                    <div class="px-6 py-3 bg-gray-50 border-t border-gray-200">
                        <button class="view-class-btn w-full py-2 text-center text-primary hover:text-blue-700 transition-all-300" data-class-id="${classObj.id}">
                            查看详情
                        </button>
                    </div>
                `;
                
                container.appendChild(card);
            });
            
            // 绑定事件
            document.querySelectorAll('.view-class-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const classId = this.getAttribute('data-class-id');
                    viewClassDetails(classId);
                });
            });
            
            document.querySelectorAll('.edit-class-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const classId = this.getAttribute('data-class-id');
                    editClass(classId);
                });
            });
            
            document.querySelectorAll('.delete-class-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const classId = this.getAttribute('data-class-id');
                    deleteClass(classId);
                });
            });
        }

        // 查看班级详情
        function viewClassDetails(classId) {
            const classObj = classes.find(c => c.id === classId);
            if (!classObj) return;
            
            // 切换到学员管理页面，并筛选当前班级
            showPage('student-management');
            document.getElementById('student-class-filter').value = classId;
            filterStudents();
        }

        // 编辑班级
        function editClass(classId) {
            const classObj = classes.find(c => c.id === classId);
            if (!classObj) return;
            
            // 填充表单
            document.getElementById('class-name').value = classObj.name;
            document.getElementById('class-type').value = classObj.type;
            
            // 修改表单提交事件
            const originalSubmitHandler = document.getElementById('add-class-form').onsubmit;
            document.getElementById('add-class-form').onsubmit = function(e) {
                e.preventDefault();
                
                // 更新班级信息
                classObj.name = document.getElementById('class-name').value;
                classObj.type = document.getElementById('class-type').value;
                
                // 保存数据
                saveData();
                
                // 更新UI
                renderClassesList();
                updateClassSelector();
                
                // 显示提示
                showToast('success', '成功', '班级信息已更新');
                
                // 关闭模态框
                document.getElementById('add-class-modal').classList.add('hidden');
                
                // 恢复原始提交事件
                document.getElementById('add-class-form').onsubmit = originalSubmitHandler;
            };
            
            // 显示模态框
            document.getElementById('add-class-modal').classList.remove('hidden');
        }

        // 删除班级
        function deleteClass(classId) {
            const classObj = classes.find(c => c.id === classId);
            if (!classObj) return;
            
            // 检查是否有关联的学员
            const classStudents = students.filter(s => s.classId === classId);
            if (classStudents.length > 0) {
                showToast('error', '错误', `无法删除班级"${classObj.name}"，该班级还有 ${classStudents.length} 名学员`);
                return;
            }
            
            // 确认删除
            if (confirm(`确定要删除班级"${classObj.name}"吗？`)) {
                // 删除相关的签到记录
                attendanceRecords = attendanceRecords.filter(r => r.classId !== classId);
                
                // 从班级列表中删除
                classes = classes.filter(c => c.id !== classId);
                
                // 如果当前选中的是这个班级，取消选中
                if (currentClass && currentClass.id === classId) {
                    currentClass = null;
                    document.getElementById('class-selector').value = '';
                    document.getElementById('current-class-name').textContent = '未选择班级';
                    document.getElementById('current-class-type').textContent = '请选择班级';
                }
                
                // 保存数据
                saveData();
                
                // 更新UI
                renderClassesList();
                updateClassSelector();
                updateDashboard();
                
                // 显示提示
                showToast('success', '成功', '班级已删除');
            }
        }

        // 添加班级
        function addClass() {
            const className = document.getElementById('class-name').value.trim();
            const classType = document.getElementById('class-type').value;
            
            if (!className) {
                showToast('error', '错误', '班级名称不能为空');
                return;
            }
            
            // 检查班级名称是否已存在
            if (classes.some(c => c.name === className)) {
                showToast('error', '错误', '班级名称已存在');
                return;
            }
            
            // 创建新班级
            const newClass = {
                id: 'class-' + Date.now(),
                name: className,
                type: classType,
                students: [],
                createTime: new Date().toISOString()
            };
            
            classes.push(newClass);
            
            // 保存数据
            saveData();
            
            // 更新UI
            renderClassesList();
            updateClassSelector();
            
            // 清空表单
            document.getElementById('add-class-form').reset();
            
            // 关闭模态框
            document.getElementById('add-class-modal').classList.add('hidden');
            
            // 显示提示
            showToast('success', '成功', '班级已添加');
        }

        // 筛选学员
        function filterStudents() {
            const searchText = document.getElementById('student-search').value.toLowerCase().trim();
            const classFilter = document.getElementById('student-class-filter').value;
            
            filteredStudents = students.filter(student => {
                // 按姓名筛选
                const nameMatch = student.name.toLowerCase().includes(searchText);
                
                // 按班级筛选
                const classMatch = !classFilter || student.classId === classFilter;
                
                return nameMatch && classMatch;
            });
            
            // 重置页码
            currentPage = 1;
            
            // 渲染学员列表
            renderStudentsList();
        }

        // 渲染学员列表
        function renderStudentsList() {
            const listContainer = document.getElementById('students-list');
            listContainer.innerHTML = '';
            
            // 计算分页
            const startIndex = (currentPage - 1) * studentsPerPage;
            const endIndex = startIndex + studentsPerPage;
            const pageStudents = filteredStudents.slice(startIndex, endIndex);
            
            // 更新总学员数
            document.getElementById('total-students').textContent = filteredStudents.length;
            
            // 更新页码
            document.getElementById('current-page').textContent = currentPage;
            
            // 更新分页按钮状态
            document.getElementById('prev-page').disabled = currentPage === 1;
            document.getElementById('next-page').disabled = endIndex >= filteredStudents.length;
            
            if (pageStudents.length === 0) {
                const row = document.createElement('tr');
                row.innerHTML = `<td colspan="7" class="px-6 py-4 text-center text-sm text-gray-500">暂无学员数据</td>`;
                listContainer.appendChild(row);
                return;
            }
            
            pageStudents.forEach((student, index) => {
                const classObj = classes.find(c => c.id === student.classId);
                const className = classObj ? classObj.name : '未知班级';
                
                // 计算出勤率
                const attendanceRate = calculateAttendanceRate(student.id);
                
                // 计算迟到次数
                const lateCount = calculateLateCount(student.id);
                
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${startIndex + index + 1}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${student.name}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${className}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${attendanceRate}%</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${lateCount}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${formatDate(new Date(student.createTime))}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">
                        <div class="flex space-x-2">
                            <button class="view-attendance-btn px-2 py-1 bg-info text-white rounded hover:bg-blue-600 transition-all-300 text-xs" data-student-id="${student.id}">
                                考勤详情
                            </button>
                            <button class="edit-student-btn px-2 py-1 bg-primary text-white rounded hover:bg-blue-600 transition-all-300 text-xs" data-student-id="${student.id}">
                                编辑
                            </button>
                            <button class="delete-student-btn px-2 py-1 bg-danger text-white rounded hover:bg-red-600 transition-all-300 text-xs" data-student-id="${student.id}">
                                删除
                            </button>
                        </div>
                    </td>
                `;
                
                listContainer.appendChild(row);
            });
            
            // 绑定事件
            document.querySelectorAll('.view-attendance-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const studentId = this.getAttribute('data-student-id');
                    viewStudentAttendance(studentId);
                });
            });
            
            document.querySelectorAll('.edit-student-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const studentId = this.getAttribute('data-student-id');
                    editStudent(studentId);
                });
            });
            
            document.querySelectorAll('.delete-student-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const studentId = this.getAttribute('data-student-id');
                    deleteStudent(studentId);
                });
            });
        }

        // 计算学员出勤率
        function calculateAttendanceRate(studentId) {
            // 获取所有签到记录
            const studentRecords = [];
            attendanceRecords.forEach(record => {
                const studentRecord = record.records.find(r => r.studentId === studentId);
                if (studentRecord) {
                    studentRecords.push(studentRecord);
                }
            });
            
            if (studentRecords.length === 0) {
                return 0;
            }
            
            // 计算出勤次数（签到和迟到）
            const attendanceCount = studentRecords.filter(r => r.status === 'present' || r.status === 'late').length;
            
            // 计算出勤率
            return Math.round((attendanceCount / studentRecords.length) * 100);
        }

        // 计算学员迟到次数
        function calculateLateCount(studentId) {
            let lateCount = 0;
            
            attendanceRecords.forEach(record => {
                const studentRecord = record.records.find(r => r.studentId === studentId);
                if (studentRecord && studentRecord.status === 'late') {
                    lateCount++;
                }
            });
            
            return lateCount;
        }

        // 查看学员考勤详情
        function viewStudentAttendance(studentId) {
            const student = students.find(s => s.id === studentId);
            if (!student) return;
            
            const classObj = classes.find(c => c.id === student.classId);
            const className = classObj ? classObj.name : '未知班级';
            
            // 设置标题
            document.getElementById('student-attendance-title').textContent = `${student.name} - ${className}`;
            
            // 获取所有签到记录
            const studentRecords = [];
            attendanceRecords.forEach(record => {
                const studentRecord = record.records.find(r => r.studentId === studentId);
                if (studentRecord) {
                    studentRecords.push({
                        date: record.date,
                        session: record.session,
                        ...studentRecord
                    });
                }
            });
            
            // 按日期排序
            studentRecords.sort((a, b) => {
                if (a.date !== b.date) {
                    return new Date(b.date) - new Date(a.date);
                }
                return a.session === 'morning' ? -1 : 1;
            });
            
            // 计算统计数据
            const totalRecords = studentRecords.length;
            const attendanceCount = studentRecords.filter(r => r.status === 'present' || r.status === 'late').length;
            const lateCount = studentRecords.filter(r => r.status === 'late').length;
            const absentCount = studentRecords.filter(r => r.status === 'absent').length;
            const leaveCount = studentRecords.filter(r => r.status === 'leave').length;
            
            // 更新统计数据
            document.getElementById('detail-attendance-count').textContent = attendanceCount;
            document.getElementById('detail-late-count').textContent = lateCount;
            document.getElementById('detail-absent-count').textContent = absentCount;
            document.getElementById('detail-leave-count').textContent = leaveCount;
            
            // 渲染考勤记录
            const detailsContainer = document.getElementById('student-attendance-details');
            detailsContainer.innerHTML = '';
            
            if (studentRecords.length === 0) {
                const row = document.createElement('tr');
                row.innerHTML = `<td colspan="5" class="px-6 py-4 text-center text-sm text-gray-500">暂无考勤记录</td>`;
                detailsContainer.appendChild(row);
            } else {
                studentRecords.forEach(record => {
                    const row = document.createElement('tr');
                    
                    let statusText = '';
                    let statusClass = '';
                    
                    switch (record.status) {
                        case 'present':
                            statusText = '签到';
                            statusClass = 'bg-green-100 text-green-800';
                            break;
                        case 'late':
                            statusText = `迟到 ${record.lateMinutes}分钟`;
                            statusClass = 'bg-yellow-100 text-yellow-800';
                            break;
                        case 'absent':
                            statusText = '旷课';
                            statusClass = 'bg-red-100 text-red-800';
                            break;
                        case 'leave':
                            statusText = '请假';
                            statusClass = 'bg-gray-100 text-gray-800';
                            break;
                    }
                    
                    row.innerHTML = `
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${formatDate(new Date(record.date))}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${record.session === 'morning' ? '上午' : '下午'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm">
                            <span class="inline-block px-2 py-1 text-xs font-semibold ${statusClass} rounded">${statusText}</span>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${record.time}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${record.remark || '-'}</td>
                    `;
                    
                    detailsContainer.appendChild(row);
                });
            }
            
            // 显示模态框
            document.getElementById('student-attendance-modal').classList.remove('hidden');
        }

        // 编辑学员
        function editStudent(studentId) {
            const student = students.find(s => s.id === studentId);
            if (!student) return;
            
            // 填充表单
            document.getElementById('edit-student-id').value = student.id;
            document.getElementById('edit-student-name').value = student.name;
            
            // 更新班级选择器
            updateEditStudentClassSelector();
            document.getElementById('edit-student-class').value = student.classId;
            
            // 显示模态框
            document.getElementById('edit-student-modal').classList.remove('hidden');
            
            // 绑定表单提交事件
            const form = document.getElementById('edit-student-form');
            const originalSubmitHandler = form.onsubmit;
            
            form.onsubmit = function(e) {
                e.preventDefault();
                
                const studentId = document.getElementById('edit-student-id').value;
                const studentName = document.getElementById('edit-student-name').value.trim();
                const studentClassId = document.getElementById('edit-student-class').value;
                
                if (!studentName) {
                    showToast('error', '错误', '学员姓名不能为空');
                    return;
                }
                
                // 检查姓名是否已存在（排除当前学员）
                if (students.some(s => s.name === studentName && s.id !== studentId)) {
                    showToast('error', '错误', '学员姓名已存在');
                    return;
                }
                
                // 更新学员信息
                const studentIndex = students.findIndex(s => s.id === studentId);
                if (studentIndex !== -1) {
                    // 如果班级变更，需要更新原班级和新班级的学员列表
                    if (students[studentIndex].classId !== studentClassId) {
                        // 从原班级中移除
                        const oldClassIndex = classes.findIndex(c => c.id === students[studentIndex].classId);
                        if (oldClassIndex !== -1) {
                            classes[oldClassIndex].students = classes[oldClassIndex].students.filter(id => id !== studentId);
                        }
                        
                        // 添加到新班级
                        const newClassIndex = classes.findIndex(c => c.id === studentClassId);
                        if (newClassIndex !== -1) {
                            classes[newClassIndex].students.push(studentId);
                        }
                    }
                    
                    // 更新学员信息
                    students[studentIndex].name = studentName;
                    students[studentIndex].classId = studentClassId;
                    
                    // 保存数据
                    saveData();
                    
                    // 更新UI
                    renderStudentsList();
                    
                    // 显示提示
                    showToast('success', '成功', '学员信息已更新');
                    
                    // 关闭模态框
                    document.getElementById('edit-student-modal').classList.add('hidden');
                    
                    // 恢复原始提交事件
                    form.onsubmit = originalSubmitHandler;
                }
            };
        }

        // 删除学员
        function deleteStudent(studentId) {
            const student = students.find(s => s.id === studentId);
            if (!student) return;
            
            // 确认删除
            if (confirm(`确定要删除学员"${student.name}"吗？`)) {
                // 从班级中移除
                const classIndex = classes.findIndex(c => c.id === student.classId);
                if (classIndex !== -1) {
                    classes[classIndex].students = classes[classIndex].students.filter(id => id !== studentId);
                }
                
                // 删除相关的签到记录
                attendanceRecords.forEach(record => {
                    record.records = record.records.filter(r => r.studentId !== studentId);
                });
                
                // 从学员列表中删除
                students = students.filter(s => s.id !== studentId);
                
                // 保存数据
                saveData();
                
                // 更新UI
                renderStudentsList();
                
                // 显示提示
                showToast('success', '成功', '学员已删除');
            }
        }

        // 添加学员
        function addStudent() {
            const studentName = document.getElementById('student-name').value.trim();
            const studentClassId = document.getElementById('student-class').value;
            
            if (!studentName) {
                showToast('error', '错误', '学员姓名不能为空');
                return;
            }
            
            if (!studentClassId) {
                showToast('error', '错误', '请选择班级');
                return;
            }
            
            // 检查姓名是否已存在
            if (students.some(s => s.name === studentName)) {
                showToast('error', '错误', '学员姓名已存在');
                return;
            }
            
            // 创建新学员
            const newStudent = {
                id: 'student-' + Date.now(),
                name: studentName,
                classId: studentClassId,
                createTime: new Date().toISOString()
            };
            
            students.push(newStudent);
            
            // 将学员添加到班级
            const classIndex = classes.findIndex(c => c.id === studentClassId);
            if (classIndex !== -1) {
                classes[classIndex].students.push(newStudent.id);
            }
            
            // 保存数据
            saveData();
            
            // 更新UI
            renderStudentsList();
            
            // 清空表单
            document.getElementById('add-student-form').reset();
            
            // 关闭模态框
            document.getElementById('add-student-modal').classList.add('hidden');
            
            // 显示提示
            showToast('success', '成功', '学员已添加');
        }

        // 批量导入学员
        function batchImportStudents(file) {
            const reader = new FileReader();
            
            reader.onload = function(e) {
                const content = e.target.result;
                const lines = content.split('\n');
                
                let successCount = 0;
                let errorCount = 0;
                const errors = [];
                
                lines.forEach((line, index) => {
                    line = line.trim();
                    if (!line) return;
                    
                    // 解析行数据，格式：姓名,学号(可选)
                    const parts = line.split(',');
                    const name = parts[0].trim();
                    
                    if (!name) {
                        errorCount++;
                        errors.push(`第${index + 1}行：姓名不能为空`);
                        return;
                    }
                    
                    // 检查姓名是否已存在
                    if (students.some(s => s.name === name)) {
                        errorCount++;
                        errors.push(`第${index + 1}行：学员"${name}"已存在`);
                        return;
                    }
                    
                    // 获取当前选中的班级
                    const classId = document.getElementById('student-class-filter').value;
                    if (!classId) {
                        errorCount++;
                        errors.push(`第${index + 1}行：请先选择班级`);
                        return;
                    }
                    
                    // 创建新学员
                    const newStudent = {
                        id: 'student-' + Date.now() + index,
                        name: name,
                        classId: classId,
                        createTime: new Date().toISOString()
                    };
                    
                    students.push(newStudent);
                    
                    // 将学员添加到班级
                    const classIndex = classes.findIndex(c => c.id === classId);
                    if (classIndex !== -1) {
                        classes[classIndex].students.push(newStudent.id);
                    }
                    
                    successCount++;
                });
                
                // 保存数据
                if (successCount > 0) {
                    saveData();
                    
                    // 更新UI
                    renderStudentsList();
                }
                
                // 显示导入结果
                let message = `成功导入 ${successCount} 名学员`;
                if (errorCount > 0) {
                    message += `，失败 ${errorCount} 名学员\n\n错误详情：\n${errors.join('\n')}`;
                }
                
                alert(message);
            };
            
            reader.readAsText(file, 'UTF-8');
        }

        // 快速签到
        function quickAttendance() {
            const date = document.getElementById('quick-date').value;
            const session = document.getElementById('quick-session').value;
            const status = document.getElementById('quick-status').value;
            
            if (!date) {
                showToast('error', '错误', '请选择日期');
                return;
            }
            
            if (!currentClass) {
                showToast('error', '错误', '请先选择班级');
                return;
            }
            
            // 检查是否已有签到记录
            let record = attendanceRecords.find(r => r.date === date && r.session === session && r.classId === currentClass.id);
            
            if (record) {
                // 确认覆盖
                if (!confirm('该日期和时段已有签到记录，确定要覆盖吗？')) {
                    return;
                }
            } else {
                // 创建新记录
                record = {
                    id: 'record-' + Date.now(),
                    date: date,
                    session: session,
                    classId: currentClass.id,
                    records: []
                };
                
                attendanceRecords.push(record);
            }
            
            // 获取班级学员
            const classStudents = students.filter(s => s.classId === currentClass.id);
            
            // 为每个学员创建签到记录
            record.records = classStudents.map(student => {
                const now = new Date();
                const time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
                
                return {
                    studentId: student.id,
                    status: status,
                    time: time,
                    lateMinutes: status === 'late' ? Math.floor(Math.random() * 30) + 1 : 0,
                    remark: status === 'leave' ? '批量签到' : ''
                };
            });
            
            // 保存数据
            saveData();
            
            // 更新仪表盘
            updateDashboard();
            
            // 显示提示
            showToast('success', '成功', '快速签到完成');
        }

        // 加载签到数据
        function loadAttendanceData() {
            const date = document.getElementById('attendance-date').value;
            const session = document.getElementById('attendance-session').value;
            const classId = document.getElementById('attendance-class').value;
            
            if (!date) {
                showToast('error', '错误', '请选择日期');
                return;
            }
            
            if (!classId) {
                showToast('error', '错误', '请选择班级');
                return;
            }
            
            // 保存当前选择
            currentAttendanceDate = date;
            currentAttendanceSession = session;
            currentAttendanceClass = classId;
            
            // 查找签到记录
            let record = attendanceRecords.find(r => r.date === date && r.session === session && r.classId === classId);
            
            // 获取班级信息
            const classObj = classes.find(c => c.id === classId);
            const className = classObj ? classObj.name : '未知班级';
            
            // 获取班级学员
            const classStudents = students.filter(s => s.classId === classId);
            
            // 如果没有签到记录，创建新记录
            if (!record) {
                record = {
                    id: 'record-' + Date.now(),
                    date: date,
                    session: session,
                    classId: classId,
                    records: []
                };
                
                // 为每个学员创建空签到记录
                classStudents.forEach(student => {
                    record.records.push({
                        studentId: student.id,
                        status: '',
                        time: '',
                        lateMinutes: 0,
                        remark: ''
                    });
                });
            }
            
            // 保存当前签到记录
            currentAttendanceRecords = record;
            
            // 更新标题
            document.getElementById('attendance-title').textContent = `${formatDate(new Date(date))} ${session === 'morning' ? '上午' : '下午'} - ${className}`;
            
            // 更新统计信息
            updateAttendanceStats();
            
            // 渲染签到列表
            renderAttendanceList();
            
            // 显示签到列表容器
            document.getElementById('attendance-list-container').classList.remove('hidden');
        }

        // 更新签到统计信息
        function updateAttendanceStats() {
            if (!currentAttendanceRecords) return;
            
            const total = currentAttendanceRecords.records.length;
            const present = currentAttendanceRecords.records.filter(r => r.status === 'present').length;
            const late = currentAttendanceRecords.records.filter(r => r.status === 'late').length;
            const absent = currentAttendanceRecords.records.filter(r => r.status === 'absent').length;
            const leave = currentAttendanceRecords.records.filter(r => r.status === 'leave').length;
            const unmarked = currentAttendanceRecords.records.filter(r => !r.status).length;
            
            document.getElementById('attendance-stats').textContent = `
                总计: ${total} | 
                签到: ${present} | 
                迟到: ${late} | 
                旷课: ${absent} | 
                请假: ${leave} | 
                未标记: ${unmarked}
            `;
        }

        // 渲染签到列表
        function renderAttendanceList() {
            if (!currentAttendanceRecords) return;
            
            const listContainer = document.getElementById('attendance-list');
            listContainer.innerHTML = '';
            
            // 获取班级学员
            const classStudents = students.filter(s => s.classId === currentAttendanceClass);
            
            // 按学员姓名排序
            classStudents.sort((a, b) => a.name.localeCompare(b.name));
            
            classStudents.forEach((student, index) => {
                // 查找学员的签到记录
                let record = currentAttendanceRecords.records.find(r => r.studentId === student.id);
                
                // 如果没有记录，创建新记录
                if (!record) {
                    record = {
                        studentId: student.id,
                        status: '',
                        time: '',
                        lateMinutes: 0,
                        remark: ''
                    };
                    currentAttendanceRecords.records.push(record);
                }
                
                const row = document.createElement('tr');
                
                // 状态选择器
                let statusSelector = `
                    <select class="attendance-status-select px-2 py-1 border border-gray-300 rounded text-sm focus:outline-none focus:ring-2 focus:ring-primary" data-student-id="${student.id}">
                        <option value="" ${!record.status ? 'selected' : ''}>未标记</option>
                        <option value="present" ${record.status === 'present' ? 'selected' : ''}>签到</option>
                        <option value="late" ${record.status === 'late' ? 'selected' : ''}>迟到</option>
                        <option value="absent" ${record.status === 'absent' ? 'selected' : ''}>旷课</option>
                        <option value="leave" ${record.status === 'leave' ? 'selected' : ''}>请假</option>
                    </select>
                `;
                
                // 迟到分钟数输入框
                let lateMinutesInput = `
                    <input type="number" class="late-minutes-input px-2 py-1 border border-gray-300 rounded text-sm focus:outline-none focus:ring-2 focus:ring-primary w-16" data-student-id="${student.id}" value="${record.lateMinutes || ''}" ${record.status !== 'late' ? 'disabled' : ''}>
                `;
                
                // 备注显示
                let remarkDisplay = record.remark ? record.remark : '';
                if (record.status === 'leave' && !record.remark) {
                    remarkDisplay = '<button class="add-leave-remark-btn text-primary hover:text-blue-700 text-sm" data-student-id="' + student.id + '">添加备注</button>';
                }
                
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${index + 1}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${student.name}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm">${statusSelector}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm">
                        <div class="flex items-center">
                            ${record.time || ''}
                            ${record.status === 'late' ? lateMinutesInput : ''}
                        </div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">${remarkDisplay}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">
                        <button class="delete-attendance-btn px-2 py-1 bg-danger text-white rounded hover:bg-red-600 transition-all-300 text-xs" data-student-id="${student.id}">
                            清除
                        </button>
                    </td>
                `;
                
                listContainer.appendChild(row);
            });
            
            // 绑定事件
            document.querySelectorAll('.attendance-status-select').forEach(select => {
                select.addEventListener('change', function() {
                    const studentId = this.getAttribute('data-student-id');
                    const status = this.value;
                    
                    // 更新签到记录
                    updateAttendanceStatus(studentId, status);
                    
                    // 更新UI
                    renderAttendanceList();
                    updateAttendanceStats();
                });
            });
            
            document.querySelectorAll('.late-minutes-input').forEach(input => {
                input.addEventListener('change', function() {
                    const studentId = this.getAttribute('data-student-id');
                    const lateMinutes = parseInt(this.value) || 0;
                    
                    // 更新迟到分钟数
                    updateLateMinutes(studentId, lateMinutes);
                });
            });
            
            document.querySelectorAll('.add-leave-remark-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const studentId = this.getAttribute('data-student-id');
                    showLeaveRemarkModal(studentId);
                });
            });
            
            document.querySelectorAll('.delete-attendance-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const studentId = this.getAttribute('data-student-id');
                    clearAttendanceRecord(studentId);
                });
            });
        }

        // 更新签到状态
        function updateAttendanceStatus(studentId, status) {
            if (!currentAttendanceRecords) return;
            
            const recordIndex = currentAttendanceRecords.records.findIndex(r => r.studentId === studentId);
            if (recordIndex !== -1) {
                // 更新状态
                currentAttendanceRecords.records[recordIndex].status = status;
                
                // 如果是签到或迟到，记录当前时间
                if (status === 'present' || status === 'late') {
                    const now = new Date();
                    currentAttendanceRecords.records[recordIndex].time = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
                    
                    // 如果是迟到，设置默认迟到分钟数
                    if (status === 'late' && !currentAttendanceRecords.records[recordIndex].lateMinutes) {
                        currentAttendanceRecords.records[recordIndex].lateMinutes = 15;
                    }
                } else {
                    currentAttendanceRecords.records[recordIndex].time = '';
                    currentAttendanceRecords.records[recordIndex].lateMinutes = 0;
                }
                
                // 如果是请假，清空备注
                if (status === 'leave') {
                    currentAttendanceRecords.records[recordIndex].remark = '';
                }
            }
        }

        // 更新迟到分钟数
        function updateLateMinutes(studentId, lateMinutes) {
            if (!currentAttendanceRecords) return;
            
            const recordIndex = currentAttendanceRecords.records.findIndex(r => r.studentId === studentId);
            if (recordIndex !== -1) {
                currentAttendanceRecords.records[recordIndex].lateMinutes = lateMinutes;
            }
        }

        // 显示请假备注模态框
        function showLeaveRemarkModal(studentId) {
            const student = students.find(s => s.id === studentId);
            if (!student) return;
            
            // 查找签到记录
            const recordIndex = currentAttendanceRecords.records.findIndex(r => r.studentId === studentId);
            if (recordIndex === -1) return;
            
            // 填充表单
            document.getElementById('leave-student-id').value = studentId;
            document.getElementById('leave-record-id').value = recordIndex;
            document.getElementById('leave-reason').value = currentAttendanceRecords.records[recordIndex].remark || '';
            
            // 显示模态框
            document.getElementById('leave-remark-modal').classList.remove('hidden');
        }

        // 保存请假备注
        function saveLeaveRemark() {
            const studentId = document.getElementById('leave-student-id').value;
            const recordIndex = parseInt(document.getElementById('leave-record-id').value);
            const reason = document.getElementById('leave-reason').value.trim();
            
            if (!reason) {
                showToast('error', '错误', '请假原因不能为空');
                return;
            }
            
            // 更新签到记录
            if (currentAttendanceRecords && recordIndex !== -1) {
                currentAttendanceRecords.records[recordIndex].remark = reason;
                
                // 更新UI
                renderAttendanceList();
                
                // 关闭模态框
                document.getElementById('leave-remark-modal').classList.add('hidden');
                
                // 显示提示
                showToast('success', '成功', '请假备注已保存');
            }
        }

        // 清除签到记录
        function clearAttendanceRecord(studentId) {
            if (!currentAttendanceRecords) return;
            
            const recordIndex = currentAttendanceRecords.records.findIndex(r => r.studentId === studentId);
            if (recordIndex !== -1) {
                currentAttendanceRecords.records[recordIndex].status = '';
                currentAttendanceRecords.records[recordIndex].time = '';
                currentAttendanceRecords.records[recordIndex].lateMinutes = 0;
                currentAttendanceRecords.records[recordIndex].remark = '';
                
                // 更新UI
                renderAttendanceList();
                updateAttendanceStats();
            }
        }

        // 批量更新签到状态
        function batchUpdateAttendance(status) {
            if (!currentAttendanceRecords) return;
            
            // 确认操作
            let confirmMessage = '';
            switch (status) {
                case 'present':
                    confirmMessage = '确定要将所有学员标记为签到吗？';
                    break;
                case 'absent':
                    confirmMessage = '确定要将所有学员标记为旷课吗？';
                    break;
                default:
                    return;
            }
            
            if (!confirm(confirmMessage)) {
                return;
            }
            
            // 更新所有学员的签到状态
            const now = new Date();
            const currentTime = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}`;
            
            currentAttendanceRecords.records.forEach(record => {
                record.status = status;
                record.time = status === 'present' ? currentTime : '';
                record.lateMinutes = 0;
                record.remark = '';
            });
            
            // 更新UI
            renderAttendanceList();
            updateAttendanceStats();
            
            // 显示提示
            showToast('success', '成功', `已将所有学员标记为${status === 'present' ? '签到' : '旷课'}`);
        }

        // 保存签到数据
        function saveAttendanceData() {
            if (!currentAttendanceRecords) return;
            
            // 检查是否有未标记的学员
            const unmarkedCount = currentAttendanceRecords.records.filter(r => !r.status).length;
            if (unmarkedCount > 0) {
                if (!confirm(`还有 ${unmarkedCount} 名学员未标记，确定要保存吗？`)) {
                    return;
                }
            }
            
            // 检查是否已存在该日期和时段的签到记录
            const existingRecordIndex = attendanceRecords.findIndex(r => 
                r.date === currentAttendanceRecords.date && 
                r.session === currentAttendanceRecords.session && 
                r.classId === currentAttendanceRecords.classId
            );
            
            if (existingRecordIndex !== -1) {
                // 更新现有记录
                attendanceRecords[existingRecordIndex] = currentAttendanceRecords;
            } else {
                // 添加新记录
                attendanceRecords.push(currentAttendanceRecords);
            }
            
            // 保存数据
            saveData();
            
            // 更新仪表盘
            updateDashboard();
            
            // 显示提示
            showToast('success', '成功', '签到数据已保存');
        }

        // 加载学员统计数据
        function loadStudentStats() {
            const classId = document.getElementById('stats-class').value;
            const studentId = document.getElementById('stats-student').value;
            const weekStart = document.getElementById('stats-week').value;
            
            if (!classId) {
                showToast('error', '错误', '请选择班级');
                return;
            }
            
            if (!studentId) {
                showToast('error', '错误', '请选择学员');
                return;
            }
            
            // 获取学员信息
            const student = students.find(s => s.id === studentId);
            if (!student) return;
            
            // 获取周次信息
            const weekStartDate = new Date(weekStart);
            const weekEndDate = new Date(weekStartDate);
            weekEndDate.setDate(weekStartDate.getDate() + 6);
            
            // 获取该周的签到记录
            const weekRecords = [];
            for (let i = 0; i < 7; i++) {
                const date = new Date(weekStartDate);
                date.setDate(weekStartDate.getDate() + i);
                const dateString = date.toISOString().split('T')[0];
                
                // 上午记录
                const morningRecord = attendanceRecords.find(r => r.date === dateString && r.session === 'morning' && r.classId === classId);
                if (morningRecord) {
                    const studentRecord = morningRecord.records.find(r => r.studentId === studentId);
                    if (studentRecord) {
                        weekRecords.push({
                            date: dateString,
                            session: 'morning',
                            ...studentRecord
                        });
                    }
                }
                
                // 下午记录
                const afternoonRecord = attendanceRecords.find(r => r.date === dateString && r.session === 'afternoon' && r.classId === classId);
                if (afternoonRecord) {
                    const studentRecord = afternoonRecord.records.find(r => r.studentId === studentId);
                    if (studentRecord) {
                        weekRecords.push({
                            date: dateString,
                            session: 'afternoon',
                            ...studentRecord
                        });
                    }
                }
            }
            
            // 计算统计数据
            const totalRecords = weekRecords.length;
            const attendanceCount = weekRecords.filter(r => r.status === 'present' || r.status === 'late').length;
            const lateCount = weekRecords.filter(r => r.status === 'late').length;
            const absentCount = weekRecords.filter(r => r.status === 'absent').length;
            const leaveCount = weekRecords.filter(r => r.status === 'leave').length;
            
            const attendanceRate = totalRecords > 0 ? Math.round((attendanceCount / totalRecords) * 100) : 0;
            
            // 更新统计数据
            document.getElementById('student-attendance-rate').textContent = `${attendanceRate}%`;
            document.getElementById('student-late-count').textContent = lateCount;
            document.getElementById('student-absent-count').textContent = absentCount;
            document.getElementById('student-leave-count').textContent = leaveCount;
            
            // 保存当前统计数据
            currentStudentStats = {
                student: student,
                weekStart: weekStartDate,
                weekEnd: weekEndDate,
                records: weekRecords,
                attendanceRate: attendanceRate,
                lateCount: lateCount,
                absentCount: absentCount,
                leaveCount: leaveCount
            };
            
            // 渲染图表
            renderStudentAttendanceChart();
            
            // 显示统计容器
            document.getElementById('student-stats-container').classList.remove('hidden');
        }

        // 渲染学员考勤图表
        function renderStudentAttendanceChart() {
            if (!currentStudentStats) return;
            
            const ctx = document.getElementById('student-attendance-chart').getContext('2d');
            
            // 准备数据
            const labels = [];
            const data = [];
            
            // 生成一周的日期标签
            for (let i = 0; i < 7; i++) {
                const date = new Date(currentStudentStats.weekStart);
                date.setDate(currentStudentStats.weekStart.getDate() + i);
                labels.push(formatDate(date, 'short'));
                
                // 上午记录
                const morningRecord = currentStudentStats.records.find(r => {
                    const recordDate = new Date(r.date);
                    return recordDate.getDate() === date.getDate() && 
                           recordDate.getMonth() === date.getMonth() && 
                           recordDate.getFullYear() === date.getFullYear() && 
                           r.session === 'morning';
                });
                
                // 下午记录
                const afternoonRecord = currentStudentStats.records.find(r => {
                    const recordDate = new Date(r.date);
                    return recordDate.getDate() === date.getDate() && 
                           recordDate.getMonth() === date.getMonth() && 
                           recordDate.getFullYear() === date.getFullYear() && 
                           r.session === 'afternoon';
                });
                
                // 计算当天的出勤率
                let dayAttendance = 0;
                let dayTotal = 0;
                
                if (morningRecord) {
                    dayTotal++;
                    if (morningRecord.status === 'present' || morningRecord.status === 'late') {
                        dayAttendance++;
                    }
                }
                
                if (afternoonRecord) {
                    dayTotal++;
                    if (afternoonRecord.status === 'present' || afternoonRecord.status === 'late') {
                        dayAttendance++;
                    }
                }
                
                data.push(dayTotal > 0 ? Math.round((dayAttendance / dayTotal) * 100) : 0);
            }
            
            // 销毁旧图表
            if (window.studentAttendanceChart) {
                window.studentAttendanceChart.destroy();
            }
            
            // 创建新图表
            window.studentAttendanceChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: '出勤率 (%)',
                        data: data,
                        backgroundColor: '#3b82f6',
                        borderColor: '#2563eb',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                callback: function(value) {
                                    return value + '%';
                                }
                            }
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: `${currentStudentStats.student.name} 周出勤率`
                        },
                        legend: {
                            display: false
                        }
                    }
                }
            });
        }

        // 生成周报表
        function generateWeekReport() {
            const weekStart = document.getElementById('week-report-select').value;
            
            if (!weekStart) {
                showToast('error', '错误', '请选择周次');
                return;
            }
            
            // 获取周次信息
            const weekStartDate = new Date(weekStart);
            const weekEndDate = new Date(weekStartDate);
            weekEndDate.setDate(weekStartDate.getDate() + 6);
            
            // 更新标题
            document.getElementById('week-report-title').textContent = `周报表: ${formatDate(weekStartDate)} 至 ${formatDate(weekEndDate)}`;
            
            // 获取所有班级的学员
            const allStudents = [...students];
            
            // 按班级和姓名排序
            allStudents.sort((a, b) => {
                if (a.classId !== b.classId) {
                    return a.classId.localeCompare(b.classId);
                }
                return a.name.localeCompare(b.name);
            });
            
            // 准备报表数据
            const reportData = [];
            
            allStudents.forEach(student => {
                const studentReport = {
                    student: student,
                    records: [],
                    attendanceCount: 0,
                    totalCount: 0,
                    attendanceRate: 0
                };
                
                // 获取该周的签到记录
                for (let i = 0; i < 5; i++) { // 只考虑工作日（周一到周五）
                    const date = new Date(weekStartDate);
                    date.setDate(weekStartDate.getDate() + i);
                    const dateString = date.toISOString().split('T')[0];
                    
                    // 上午记录
                    const morningRecord = attendanceRecords.find(r => r.date === dateString && r.session === 'morning' && r.classId === student.classId);
                    if (morningRecord) {
                        const studentRecord = morningRecord.records.find(r => r.studentId === student.id);
                        if (studentRecord) {
                            studentReport.records.push({
                                date: dateString,
                                session: 'morning',
                                ...studentRecord
                            });
                            studentReport.totalCount++;
                            if (studentRecord.status === 'present' || studentRecord.status === 'late') {
                                studentReport.attendanceCount++;
                            }
                        }
                    }
                    
                    // 下午记录
                    const afternoonRecord = attendanceRecords.find(r => r.date === dateString && r.session === 'afternoon' && r.classId === student.classId);
                    if (afternoonRecord) {
                        const studentRecord = afternoonRecord.records.find(r => r.studentId === student.id);
                        if (studentRecord) {
                            studentReport.records.push({
                                date: dateString,
                                session: 'afternoon',
                                ...studentRecord
                            });
                            studentReport.totalCount++;
                            if (studentRecord.status === 'present' || studentRecord.status === 'late') {
                                studentReport.attendanceCount++;
                            }
                        }
                    }
                }
                
                // 计算出勤率
                studentReport.attendanceRate = studentReport.totalCount > 0 ? 
                    Math.round((studentReport.attendanceCount / studentReport.totalCount) * 100) : 0;
                
                reportData.push(studentReport);
            });
            
            // 保存当前报表数据
            currentWeekReport = {
                weekStart: weekStartDate,
                weekEnd: weekEndDate,
                data: reportData
            };
            
            // 渲染报表
            renderWeekReport();
            
            // 显示报表容器
            document.getElementById('week-report-container').classList.remove('hidden');
        }

        // 渲染周报表
        function renderWeekReport() {
            if (!currentWeekReport) return;
            
            const reportBody = document.getElementById('week-report-body');
            reportBody.innerHTML = '';
            
            currentWeekReport.data.forEach(report => {
                const row = document.createElement('tr');
                
                // 获取班级信息
                const classObj = classes.find(c => c.id === report.student.classId);
                const className = classObj ? classObj.name : '未知班级';
                
                // 准备每天的签到状态
                const statusCells = [];
                
                // 只考虑工作日（周一到周五）
                for (let i = 0; i < 5; i++) {
                    const date = new Date(currentWeekReport.weekStart);
                    date.setDate(currentWeekReport.weekStart.getDate() + i);
                    const dateString = date.toISOString().split('T')[0];
                    
                    // 上午记录
                    const morningRecord = report.records.find(r => r.date === dateString && r.session === 'morning');
                    let morningStatus = '';
                    let morningClass = '';
                    
                    if (morningRecord) {
                        switch (morningRecord.status) {
                            case 'present':
                                morningStatus = '签到';
                                morningClass = 'bg-green-100 text-green-800';
                                break;
                            case 'late':
                                morningStatus = `迟到${morningRecord.lateMinutes}分`;
                                morningClass = 'bg-yellow-100 text-yellow-800';
                                break;
                            case 'absent':
                                morningStatus = '旷课';
                                morningClass = 'bg-red-100 text-red-800';
                                break;
                            case 'leave':
                                morningStatus = '请假';
                                morningClass = 'bg-gray-100 text-gray-800';
                                break;
                        }
                    }
                    
                    // 下午记录
                    const afternoonRecord = report.records.find(r => r.date === dateString && r.session === 'afternoon');
                    let afternoonStatus = '';
                    let afternoonClass = '';
                    
                    if (afternoonRecord) {
                        switch (afternoonRecord.status) {
                            case 'present':
                                afternoonStatus = '签到';
                                afternoonClass = 'bg-green-100 text-green-800';
                                break;
                            case 'late':
                                afternoonStatus = `迟到${afternoonRecord.lateMinutes}分`;
                                afternoonClass = 'bg-yellow-100 text-yellow-800';
                                break;
                            case 'absent':
                                afternoonStatus = '旷课';
                                afternoonClass = 'bg-red-100 text-red-800';
                                break;
                            case 'leave':
                                afternoonStatus = '请假';
                                afternoonClass = 'bg-gray-100 text-gray-800';
                                break;
                        }
                    }
                    
                    statusCells.push(`
                        <td class="px-6 py-4 whitespace-nowrap text-sm">
                            <span class="inline-block px-2 py-1 text-xs font-semibold ${morningClass} rounded">${morningStatus}</span>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm">
                            <span class="inline-block px-2 py-1 text-xs font-semibold ${afternoonClass} rounded">${afternoonStatus}</span>
                        </td>
                    `);
                }
                
                row.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-800">
                        <div>
                            <div class="font-medium">${report.student.name}</div>
                            <div class="text-xs text-gray-500">${className}</div>
                        </div>
                    </td>
                    ${statusCells.join('')}
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-800">${report.attendanceRate}%</td>
                `;
                
                reportBody.appendChild(row);
            });
        }

        // 导出周报表
        function exportWeekReport() {
            if (!currentWeekReport) return;
            
            // 准备CSV数据
            let csvContent = "姓名,班级,周一上午,周一下午,周二上午,周二下午,周三上午,周三下午,周四上午,周四下午,周五上午,周五下午,出勤率\n";
            
            currentWeekReport.data.forEach(report => {
                const classObj = classes.find(c => c.id === report.student.classId);
                const className = classObj ? classObj.name : '未知班级';
                
                // 准备每天的签到状态
                const statusValues = [];
                
                // 只考虑工作日（周一到周五）
                for (let i = 0; i < 5; i++) {
                    const date = new Date(currentWeekReport.weekStart);
                    date.setDate(currentWeekReport.weekStart.getDate() + i);
                    const dateString = date.toISOString().split('T')[0];
                    
                    // 上午记录
                    const morningRecord = report.records.find(r => r.date === dateString && r.session === 'morning');
                    let morningStatus = '';
                    
                    if (morningRecord) {
                        switch (morningRecord.status) {
                            case 'present':
                                morningStatus = '签到';
                                break;
                            case 'late':
                                morningStatus = `迟到${morningRecord.lateMinutes}分`;
                                break;
                            case 'absent':
                                morningStatus = '旷课';
                                break;
                            case 'leave':
                                morningStatus = '请假';
                                break;
                        }
                    }
                    
                    // 下午记录
                    const afternoonRecord = report.records.find(r => r.date === dateString && r.session === 'afternoon');
                    let afternoonStatus = '';
                    
                    if (afternoonRecord) {
                        switch (afternoonRecord.status) {
                            case 'present':
                                afternoonStatus = '签到';
                                break;
                            case 'late':
                                afternoonStatus = `迟到${afternoonRecord.lateMinutes}分`;
                                break;
                            case 'absent':
                                afternoonStatus = '旷课';
                                break;
                            case 'leave':
                                afternoonStatus = '请假';
                                break;
                        }
                    }
                    
                    statusValues.push(morningStatus, afternoonStatus);
                }
                
                csvContent += `"${report.student.name}","${className}",${statusValues.join(',')},${report.attendanceRate}%\n`;
            });
            
            // 创建下载链接
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.setAttribute('href', url);
            link.setAttribute('download', `周报表_${formatDate(currentWeekReport.weekStart, 'file')}.csv`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            // 显示提示
            showToast('success', '成功', '周报表已导出');
        }

        // 导出数据
        function exportData() {
            const data = {
                classes: classes,
                students: students,
                attendanceRecords: attendanceRecords,
                exportDate: new Date().toISOString()
            };
            
            const jsonString = JSON.stringify(data, null, 2);
            const blob = new Blob([jsonString], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.setAttribute('href', url);
            link.setAttribute('download', `考勤数据_${formatDate(new Date(), 'file')}.json`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            // 显示提示
            showToast('success', '成功', '数据已导出');
        }

        // 导入数据
        function importData(file) {
            const reader = new FileReader();
            
            reader.onload = function(e) {
                try {
                    const data = JSON.parse(e.target.result);
                    
                    // 验证数据格式
                    if (!data.classes || !data.students || !Array.isArray(data.classes) || !Array.isArray(data.students)) {
                        throw new Error('数据格式不正确');
                    }
                    
                    // 确认导入
                    if (confirm('导入数据将覆盖当前所有数据，确定要继续吗？')) {
                        // 更新数据
                        classes = data.classes || [];
                        students = data.students || [];
                        attendanceRecords = data.attendanceRecords || [];
                        
                        // 保存数据
                        saveData();
                        
                        // 更新UI
                        updateClassSelector();
                        updateDashboard();
                        
                        // 显示提示
                        showToast('success', '成功', '数据已导入');
                        
                        // 刷新当前页面
                        location.reload();
                    }
                } catch (error) {
                    showToast('error', '错误', '导入失败：' + error.message);
                }
            };
            
            reader.readAsText(file, 'UTF-8');
        }

        // 渲染图表
        function renderCharts() {
            renderClassAttendanceChart();
            renderAttendanceStatusChart();
        }

        // 渲染班级考勤概览图表
        function renderClassAttendanceChart() {
            const ctx = document.getElementById('class-attendance-chart').getContext('2d');
            
            // 准备数据
            const labels = [];
            const data = [];
            
            classes.forEach(classObj => {
                labels.push(classObj.name);
                
                // 计算班级出勤率
                let totalCount = 0;
                let attendanceCount = 0;
                
                // 获取最近30天的日期
                const thirtyDaysAgo = new Date();
                thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30);
                
                // 筛选最近30天的签到记录
                const recentRecords = attendanceRecords.filter(r => {
                    const recordDate = new Date(r.date);
                    return r.classId === classObj.id && recordDate >= thirtyDaysAgo;
                });
                
                // 计算出勤率
                recentRecords.forEach(record => {
                    record.records.forEach(studentRecord => {
                        totalCount++;
                        if (studentRecord.status === 'present' || studentRecord.status === 'late') {
                            attendanceCount++;
                        }
                    });
                });
                
                const attendanceRate = totalCount > 0 ? Math.round((attendanceCount / totalCount) * 100) : 0;
                data.push(attendanceRate);
            });
            
            // 销毁旧图表
            if (window.classAttendanceChart) {
                window.classAttendanceChart.destroy();
            }
            
            // 创建新图表
            window.classAttendanceChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: '出勤率 (%)',
                        data: data,
                        backgroundColor: '#3b82f6',
                        borderColor: '#2563eb',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                callback: function(value) {
                                    return value + '%';
                                }
                            }
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: '班级出勤率概览（最近30天）'
                        }
                    }
                }
            });
        }

        // 渲染考勤状态分布图表
        function renderAttendanceStatusChart() {
            const ctx = document.getElementById('attendance-status-chart').getContext('2d');
            
            // 获取最近30天的日期
            const thirtyDaysAgo = new Date();
            thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30);
            
            // 筛选最近30天的签到记录
            const recentRecords = attendanceRecords.filter(r => {
                const recordDate = new Date(r.date);
                return recordDate >= thirtyDaysAgo;
            });
            
            // 计算各状态的数量
            let presentCount = 0;
            let lateCount = 0;
            let absentCount = 0;
            let leaveCount = 0;
            
            recentRecords.forEach(record => {
                record.records.forEach(studentRecord => {
                    switch (studentRecord.status) {
                        case 'present':
                            presentCount++;
                            break;
                        case 'late':
                            lateCount++;
                            break;
                        case 'absent':
                            absentCount++;
                            break;
                        case 'leave':
                            leaveCount++;
                            break;
                    }
                });
            });
            
            // 销毁旧图表
            if (window.attendanceStatusChart) {
                window.attendanceStatusChart.destroy();
            }
            
            // 创建新图表
            window.attendanceStatusChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['签到', '迟到', '旷课', '请假'],
                    datasets: [{
                        data: [presentCount, lateCount, absentCount, leaveCount],
                        backgroundColor: [
                            '#10b981', // 绿色 - 签到
                            '#f59e0b', // 黄色 - 迟到
                            '#ef4444', // 红色 - 旷课
                            '#64748b'  // 灰色 - 请假
                        ],
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        title: {
                            display: true,
                            text: '考勤状态分布（最近30天）'
                        },
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });
        }

        // 显示提示消息
        function showToast(type, title, message) {
            const toast = document.getElementById('toast');
            const toastIcon = document.getElementById('toast-icon');
            const toastTitle = document.getElementById('toast-title');
            const toastMessage = document.getElementById('toast-message');
            
            // 设置图标和颜色
            switch (type) {
                case 'success':
                    toastIcon.className = 'mr-3 text-xl text-success';
                    toastIcon.innerHTML = '<i class="fa fa-check-circle"></i>';
                    break;
                case 'error':
                    toastIcon.className = 'mr-3 text-xl text-danger';
                    toastIcon.innerHTML = '<i class="fa fa-times-circle"></i>';
                    break;
                case 'warning':
                    toastIcon.className = 'mr-3 text-xl text-warning';
                    toastIcon.innerHTML = '<i class="fa fa-exclamation-triangle"></i>';
                    break;
                case 'info':
                    toastIcon.className = 'mr-3 text-xl text-info';
                    toastIcon.innerHTML = '<i class="fa fa-info-circle"></i>';
                    break;
            }
            
            // 设置标题和消息
            toastTitle.textContent = title;
            toastMessage.textContent = message;
            
            // 显示提示
            toast.classList.remove('translate-y-10', 'opacity-0');
            
            // 3秒后隐藏
            setTimeout(() => {
                toast.classList.add('translate-y-10', 'opacity-0');
            }, 3000);
        }

        // 格式化日期
        function formatDate(date, format = 'full') {
            const year = date.getFullYear();
            const month = (date.getMonth() + 1).toString().padStart(2, '0');
            const day = date.getDate().toString().padStart(2, '0');
            
            switch (format) {
                case 'short':
                    return `${month}-${day}`;
                case 'file':
                    return `${year}${month}${day}`;
                case 'full':
                default:
                    return `${year}-${month}-${day}`;
            }
        }

        // 当班级选择器变更时，更新学员选择器
        document.getElementById('stats-class').addEventListener('change', function() {
            const classId = this.value;
            const studentSelector = document.getElementById('stats-student');
            
            // 清空现有选项
            studentSelector.innerHTML = '<option value="">选择学员</option>';
            
            if (classId) {
                // 获取班级学员
                const classStudents = students.filter(s => s.classId === classId);
                
                // 添加学员选项
                classStudents.forEach(student => {
                    const option = document.createElement('option');
                    option.value = student.id;
                    option.textContent = student.name;
                    studentSelector.appendChild(option);
                });
            }
            
            // 隐藏统计容器
            document.getElementById('student-stats-container').classList.add('hidden');
        });
    </script>
</body>
</html>
