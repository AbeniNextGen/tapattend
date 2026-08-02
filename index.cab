import React, { useState, useEffect, useRef, useMemo } from 'react';

const INITIAL_DEPARTMENTS = [
  { id: 'DEP-01', name: 'ICT & Infrastructure', code: 'ICT', description: 'Systems, network engineering, and enterprise software.' },
  { id: 'DEP-02', name: 'Administration', code: 'ADM', description: 'General operations, executive assistance, and logistics.' },
  { id: 'DEP-03', name: 'Finance & Accounting', code: 'FIN', description: 'Financial management, compliance, and accounts.' },
  { id: 'DEP-04', name: 'Human Resources', code: 'HR', description: 'Talent acquisition, relations, and staff development.' },
  { id: 'DEP-05', name: 'Operations & Supply', code: 'OPS', description: 'Facility maintenance, procurement, and daily execution.' }
];

const INITIAL_EMPLOYEES = [
  {
    id: 'EMP-1001',
    fullName: 'Alexander Wright',
    department: 'ICT & Infrastructure',
    phone: '+1 (555) 234-5678',
    nfcUid: '04:A2:89:FE:32',
    barcode: 'TAP-1001-88',
    status: 'Active',
    avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=150&auto=format&fit=crop&q=80'
  },
  {
    id: 'EMP-1002',
    fullName: 'Sophia Martinez',
    department: 'Finance & Accounting',
    phone: '+1 (555) 987-6543',
    nfcUid: '04:B1:77:C3:90',
    barcode: 'TAP-1002-99',
    status: 'Active',
    avatar: 'https://images.unsplash.com/photo-1580489944761-15a19d654956?w=150&auto=format&fit=crop&q=80'
  },
  {
    id: 'EMP-1003',
    fullName: 'David Chen',
    department: 'ICT & Infrastructure',
    phone: '+1 (555) 456-7890',
    nfcUid: '04:F9:12:44:09',
    barcode: 'TAP-1003-44',
    status: 'Active',
    avatar: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=150&auto=format&fit=crop&q=80'
  },
  {
    id: 'EMP-1004',
    fullName: 'Elena Rostova',
    department: 'Administration',
    phone: '+1 (555) 321-6549',
    nfcUid: '04:C8:44:81:11',
    barcode: 'TAP-1004-12',
    status: 'Active',
    avatar: 'https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?w=150&auto=format&fit=crop&q=80'
  },
  {
    id: 'EMP-1005',
    fullName: 'Marcus Vance',
    department: 'Operations & Supply',
    phone: '+1 (555) 654-9870',
    nfcUid: '04:99:EE:21:77',
    barcode: 'TAP-1005-77',
    status: 'Active',
    avatar: 'https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=150&auto=format&fit=crop&q=80'
  },
  {
    id: 'EMP-1006',
    fullName: 'Amara Okafor',
    department: 'Human Resources',
    phone: '+1 (555) 789-0123',
    nfcUid: '04:D3:55:12:88',
    barcode: 'TAP-1006-33',
    status: 'Active',
    avatar: 'https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?w=150&auto=format&fit=crop&q=80'
  }
];

// Helper date formatting string
const getFormattedDate = (offsetDays = 0) => {
  const d = new Date();
  d.setDate(d.getDate() - offsetDays);
  return d.toISOString().split('T')[0];
};

const INITIAL_ATTENDANCE = [
  {
    id: 'ATT-9001',
    employeeId: 'EMP-1001',
    fullName: 'Alexander Wright',
    department: 'ICT & Infrastructure',
    phone: '+1 (555) 234-5678',
    date: getFormattedDate(0),
    time: '08:14 AM',
    method: 'NFC',
    status: 'On Time',
    timestamp: Date.now() - 3600000 * 2
  },
  {
    id: 'ATT-9002',
    employeeId: 'EMP-1002',
    fullName: 'Sophia Martinez',
    department: 'Finance & Accounting',
    phone: '+1 (555) 987-6543',
    date: getFormattedDate(0),
    time: '08:28 AM',
    method: 'Barcode',
    status: 'On Time',
    timestamp: Date.now() - 3600000 * 1.5
  },
  {
    id: 'ATT-9003',
    employeeId: 'EMP-1003',
    fullName: 'David Chen',
    department: 'ICT & Infrastructure',
    phone: '+1 (555) 456-7890',
    date: getFormattedDate(0),
    time: '09:05 AM',
    method: 'NFC',
    status: 'Late',
    timestamp: Date.now() - 3600000 * 0.8
  },
  {
    id: 'ATT-8990',
    employeeId: 'EMP-1004',
    fullName: 'Elena Rostova',
    department: 'Administration',
    phone: '+1 (555) 321-6549',
    date: getFormattedDate(1),
    time: '08:02 AM',
    method: 'NFC',
    status: 'On Time',
    timestamp: Date.now() - 86400000 - 3600000 * 3
  },
  {
    id: 'ATT-8991',
    employeeId: 'EMP-1005',
    fullName: 'Marcus Vance',
    department: 'Operations & Supply',
    phone: '+1 (555) 654-9870',
    date: getFormattedDate(1),
    time: '08:45 AM',
    method: 'Barcode',
    status: 'On Time',
    timestamp: Date.now() - 86400000 - 3600000 * 2.5
  }
];

const INITIAL_SETTINGS = {
  orgName: 'ExSofós Technologies Ltd.',
  orgLogo: 'https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=100&auto=format&fit=crop&q=80',
  timezone: 'UTC +03:00 (East Africa Time)',
  startTime: '08:30',
  endTime: '17:00',
  enableBarcodeBackup: true,
  autoScanInterval: 3,
  playSoundFeedback: true
};

const playAudioFeedback = (type = 'success') => {
  try {
    const AudioCtx = window.AudioContext || window.webkitAudioContext;
    if (!AudioCtx) return;
    const ctx = new AudioCtx();
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.connect(gain);
    gain.connect(ctx.destination);

    if (type === 'success') {
      osc.type = 'sine';
      osc.frequency.setValueAtTime(523.25, ctx.currentTime); // C5
      osc.frequency.exponentialRampToValueAtTime(1046.50, ctx.currentTime + 0.15); // C6
      gain.gain.setValueAtTime(0.15, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.25);
      osc.start();
      osc.stop(ctx.currentTime + 0.25);
    } else if (type === 'duplicate') {
      osc.type = 'sawtooth';
      osc.frequency.setValueAtTime(300, ctx.currentTime);
      osc.frequency.setValueAtTime(200, ctx.currentTime + 0.12);
      gain.gain.setValueAtTime(0.2, ctx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.3);
      osc.start();
      osc.stop(ctx.currentTime + 0.3);
    }
  } catch (e) {
    console.error('Audio feedback error:', e);
  }
};

export default function App() {
  // Navigation & Authentication
  const [isAuthenticated, setIsAuthenticated] = useState(true);
  const [currentTab, setCurrentTab] = useState('dashboard');
  const [isDarkMode, setIsDarkMode] = useState(false);
  const [isMobileMenuOpen, setIsMobileMenuOpen] = useState(false);

  // App Master Data
  const [departments, setDepartments] = useState(INITIAL_DEPARTMENTS);
  const [employees, setEmployees] = useState(INITIAL_EMPLOYEES);
  const [attendance, setAttendance] = useState(INITIAL_ATTENDANCE);
  const [settings, setSettings] = useState(INITIAL_SETTINGS);

  // System Toast Notifications
  const [toast, setToast] = useState(null);

  const showToast = (message, type = 'info') => {
    setToast({ message, type, id: Date.now() });
    setTimeout(() => {
      setToast(null);
    }, 4000);
  };

  // Live Clock state
  const [time, setTime] = useState(new Date());

  useEffect(() => {
    const timer = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(timer);
  }, []);

  // Theme Toggling
  useEffect(() => {
    if (isDarkMode) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }, [isDarkMode]);

  if (!isAuthenticated) {
    return (
      <LoginScreen
        onLogin={() => {
          setIsAuthenticated(true);
          showToast('Welcome back, Administrator!', 'success');
        }}
      />
    );
  }

  return (
    <div className={`min-h-screen font-sans ${isDarkMode ? 'dark bg-slate-950 text-slate-100' : 'bg-slate-50 text-slate-800'} transition-colors duration-200 flex flex-col md:flex-row`}>
      
      {/* Toast Overlay */}
      {toast && (
        <div className="fixed top-5 right-5 z-50 animate-bounce transition-all">
          <div className={`px-4 py-3 rounded-lg shadow-lg flex items-center space-x-3 text-sm font-medium border ${
            toast.type === 'success' ? 'bg-emerald-600 border-emerald-500 text-white' :
            toast.type === 'error' ? 'bg-rose-600 border-rose-500 text-white' :
            toast.type === 'warning' ? 'bg-amber-500 border-amber-400 text-slate-900' :
            'bg-blue-600 border-blue-500 text-white'
          }`}>
            <svg className="w-5 h-5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            <span>{toast.message}</span>
          </div>
        </div>
      )}

      {}
      <aside className={`md:w-64 flex-shrink-0 border-r ${isDarkMode ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200'} flex flex-col justify-between z-30 transition-all`}>
        <div>
          {/* Header Branding */}
          <div className="p-6 border-b border-slate-200 dark:border-slate-800 flex items-center justify-between">
            <div className="flex items-center space-x-3">
              <div className="w-10 h-10 rounded-xl bg-gradient-to-tr from-blue-700 via-blue-600 to-indigo-500 flex items-center justify-center text-white font-bold shadow-md shadow-blue-500/20">
                <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 11c0 3.517-1.009 6.799-2.753 9.571m-3.44-2.04l.054-.09A13.916 13.916 0 008 11a4 4 0 118 0c0 1.017-.07 2.019-.203 3m-2.118 6.844A21.88 21.88 0 0015.171 17m3.839 1.132c.645-2.266.99-4.659.99-7.132A8 8 0 008 4.07M3 15.364c.64-1.319 1-2.8 1-4.364 0-1.457-.39-2.823-1.07-4" />
                </svg>
              </div>
              <div>
                <h1 className="font-extrabold text-lg text-slate-900 dark:text-white tracking-tight leading-none">TapAttend</h1>
                <p className="text-[10px] font-semibold text-blue-600 dark:text-blue-400 tracking-wider uppercase mt-1">ExSofós Platform</p>
              </div>
            </div>

            {/* Mobile Hamburger toggle */}
            <button 
              onClick={() => setIsMobileMenuOpen(!isMobileMenuOpen)}
              className="md:hidden p-2 text-slate-500 hover:text-slate-700 dark:hover:text-slate-200"
            >
              <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d={isMobileMenuOpen ? "M6 18L18 6M6 6l12 12" : "M4 6h16M4 12h16M4 18h16"} />
              </svg>
            </button>
          </div>

          {/* Navigation Items */}
          <nav className={`p-4 space-y-1 ${isMobileMenuOpen ? 'block' : 'hidden md:block'}`}>
            <NavItem 
              icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6zM14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6zM4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2zM14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />}
              label="Dashboard"
              active={currentTab === 'dashboard'}
              onClick={() => { setCurrentTab('dashboard'); setIsMobileMenuOpen(false); }}
            />
            <NavItem 
              icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z" />}
              label="Attendance Scanner"
              badge="LIVE"
              active={currentTab === 'scanner'}
              onClick={() => { setCurrentTab('scanner'); setIsMobileMenuOpen(false); }}
            />
            <NavItem 
              icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" />}
              label="Attendance Records"
              active={currentTab === 'records'}
              onClick={() => { setCurrentTab('records'); setIsMobileMenuOpen(false); }}
            />
            <NavItem 
              icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5m0 0h5m-5 0V11m0 0h-5m5 0h5" />}
              label="Departments"
              active={currentTab === 'departments'}
              onClick={() => { setCurrentTab('departments'); setIsMobileMenuOpen(false); }}
            />
            <NavItem 
              icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z" />}
              label="Employees"
              active={currentTab === 'employees'}
              onClick={() => { setCurrentTab('employees'); setIsMobileMenuOpen(false); }}
            />
            <NavItem 
              icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />}
              label="Settings"
              active={currentTab === 'settings'}
              onClick={() => { setCurrentTab('settings'); setIsMobileMenuOpen(false); }}
            />
          </nav>
        </div>

        {/* Footer Admin info & mode toggles */}
        <div className={`p-4 border-t ${isDarkMode ? 'border-slate-800' : 'border-slate-200'} space-y-3`}>
          <div className="flex items-center justify-between px-2">
            <span className="text-xs font-semibold text-slate-500 uppercase tracking-wider">Appearance</span>
            <button
              onClick={() => setIsDarkMode(!isDarkMode)}
              className="p-1.5 rounded-lg bg-slate-200 dark:bg-slate-800 text-slate-700 dark:text-slate-300 hover:bg-slate-300 dark:hover:bg-slate-700 transition"
              title="Toggle Dark Mode"
            >
              {isDarkMode ? (
                <svg className="w-4 h-4 text-amber-400" fill="currentColor" viewBox="0 0 20 20">
                  <path fillRule="evenodd" d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 100 2h1z" clipRule="evenodd" />
                </svg>
              ) : (
                <svg className="w-4 h-4 text-slate-600" fill="currentColor" viewBox="0 0 20 20">
                  <path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
                </svg>
              )}
            </button>
          </div>

          <div className="flex items-center space-x-3 p-2 rounded-xl bg-slate-100 dark:bg-slate-800/60">
            <div className="w-9 h-9 rounded-full bg-blue-600 text-white flex items-center justify-center font-bold text-sm shadow">
              AD
            </div>
            <div className="flex-1 min-w-0">
              <p className="text-xs font-bold truncate text-slate-800 dark:text-slate-200">System Admin</p>
              <p className="text-[10px] text-slate-500 truncate">admin@tapattend.com</p>
            </div>
            <button 
              onClick={() => {
                setIsAuthenticated(false);
                showToast('Logged out successfully', 'info');
              }}
              title="Logout" 
              className="p-1.5 text-slate-400 hover:text-rose-500 dark:hover:text-rose-400 transition"
            >
              <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
              </svg>
            </button>
          </div>
        </div>
      </aside>

      {/* Main Content Area */}
      <main className="flex-1 flex flex-col min-w-0 overflow-y-auto min-h-screen">
        
        {/* Top Navbar */}
        <header className={`px-6 py-4 border-b ${isDarkMode ? 'bg-slate-900/80 border-slate-800' : 'bg-white/80 border-slate-200'} backdrop-blur sticky top-0 z-20 flex items-center justify-between`}>
          <div>
            <h2 className="text-xl font-bold tracking-tight text-slate-800 dark:text-slate-100 capitalize">
              {currentTab === 'dashboard' ? 'Overview Dashboard' :
               currentTab === 'scanner' ? 'NFC & Barcode Attendance Terminal' :
               currentTab === 'records' ? 'Attendance Logs & Reports' :
               currentTab === 'departments' ? 'Department Management' :
               currentTab === 'employees' ? 'Employee Roster & Credentials' : 'System Settings'}
            </h2>
            <p className="text-xs text-slate-500 dark:text-slate-400">
              {settings.orgName} • Enterprise Attendance Tracking
            </p>
          </div>

          <div className="flex items-center space-x-4">
            {/* Live Time Display */}
            <div className="hidden sm:flex items-center space-x-2 px-3 py-1.5 rounded-lg bg-blue-50 dark:bg-slate-800 border border-blue-100 dark:border-slate-700 text-blue-700 dark:text-blue-400 font-mono text-xs font-semibold">
              <span className="w-2 h-2 rounded-full bg-emerald-500 animate-ping" />
              <span>{time.toLocaleDateString(undefined, { weekday: 'short', month: 'short', day: 'numeric' })}</span>
              <span>•</span>
              <span className="font-bold">{time.toLocaleTimeString()}</span>
            </div>

            <button
              onClick={() => setCurrentTab('scanner')}
              className="flex items-center space-x-2 px-4 py-2 bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white rounded-lg text-sm font-semibold shadow-md shadow-blue-500/20 transition active:scale-95"
            >
              <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z" />
              </svg>
              <span>Launch Terminal</span>
            </button>
          </div>
        </header>

        {/* Dynamic Views */}
        <div className="p-6 flex-1 max-w-7xl w-full mx-auto">
          {currentTab === 'dashboard' && (
            <DashboardView 
              employees={employees} 
              attendance={attendance} 
              departments={departments}
              settings={settings}
              onNavigate={(tab) => setCurrentTab(tab)}
            />
          )}

          {currentTab === 'scanner' && (
            <ScannerView 
              employees={employees}
              attendance={attendance}
              setAttendance={setAttendance}
              settings={settings}
              showToast={showToast}
            />
          )}

          {currentTab === 'records' && (
            <AttendanceRecordsView 
              attendance={attendance}
              employees={employees}
              departments={departments}
              settings={settings}
            />
          )}

          {currentTab === 'departments' && (
            <DepartmentsView 
              departments={departments}
              setDepartments={setDepartments}
              employees={employees}
              showToast={showToast}
            />
          )}

          {currentTab === 'employees' && (
            <EmployeesView 
              employees={employees}
              setEmployees={setEmployees}
              departments={departments}
              showToast={showToast}
            />
          )}

          {currentTab === 'settings' && (
            <SettingsView 
              settings={settings}
              setSettings={setSettings}
              showToast={showToast}
            />
          )}
        </div>
      </main>
    </div>
  );
}

// Navigation Item helper component
function NavItem({ icon, label, active, onClick, badge }) {
  return (
    <button
      onClick={onClick}
      className={`w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl text-sm font-medium transition-all ${
        active 
          ? 'bg-blue-600 text-white shadow-md shadow-blue-500/20 font-semibold' 
          : 'text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800/80 hover:text-slate-900 dark:hover:text-slate-100'
      }`}
    >
      <div className="flex items-center space-x-3">
        <svg className="w-5 h-5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          {icon}
        </svg>
        <span className="truncate">{label}</span>
      </div>
      {badge && (
        <span className={`text-[10px] font-black px-1.5 py-0.5 rounded ${active ? 'bg-white text-blue-700' : 'bg-blue-100 dark:bg-blue-900/60 text-blue-700 dark:text-blue-300'}`}>
          {badge}
        </span>
      )}
    </button>
  );
}

function LoginScreen({ onLogin }) {
  const [email, setEmail] = useState('admin@tapattend.com');
  const [password, setPassword] = useState('admin123');
  const [error, setError] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (email === 'admin@tapattend.com' && password === 'admin123') {
      onLogin();
    } else {
      setError('Invalid admin credentials. Use admin@tapattend.com / admin123');
    }
  };

  return (
    <div className="min-h-screen bg-slate-900 flex items-center justify-center p-4">
      <div className="max-w-md w-full bg-slate-800 rounded-2xl shadow-2xl border border-slate-700 p-8 space-y-6">
        <div className="text-center space-y-2">
          <div className="mx-auto w-14 h-14 bg-gradient-to-tr from-blue-600 to-indigo-500 rounded-2xl flex items-center justify-center text-white shadow-lg shadow-blue-500/30">
            <svg className="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 11c0 3.517-1.009 6.799-2.753 9.571m-3.44-2.04l.054-.09A13.916 13.916 0 008 11a4 4 0 118 0c0 1.017-.07 2.019-.203 3m-2.118 6.844A21.88 21.88 0 0015.171 17m3.839 1.132c.645-2.266.99-4.659.99-7.132A8 8 0 008 4.07M3 15.364c.64-1.319 1-2.8 1-4.364 0-1.457-.39-2.823-1.07-4" />
            </svg>
          </div>
          <h2 className="text-2xl font-bold text-white tracking-tight">TapAttend Console</h2>
          <p className="text-xs text-slate-400">ExSofós Enterprise NFC & Barcode Management</p>
        </div>

        {error && (
          <div className="p-3 rounded-lg bg-rose-500/10 border border-rose-500/30 text-rose-300 text-xs text-center font-medium">
            {error}
          </div>
        )}

        <form onSubmit={handleSubmit} className="space-y-4">
          <div>
            <label className="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1">Admin Email</label>
            <input 
              type="email" 
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
              className="w-full px-4 py-2.5 rounded-xl bg-slate-900 border border-slate-700 text-white placeholder-slate-500 focus:outline-none focus:border-blue-500 text-sm"
              placeholder="admin@tapattend.com"
            />
          </div>

          <div>
            <label className="block text-xs font-semibold text-slate-300 uppercase tracking-wider mb-1">Password</label>
            <input 
              type="password" 
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
              className="w-full px-4 py-2.5 rounded-xl bg-slate-900 border border-slate-700 text-white placeholder-slate-500 focus:outline-none focus:border-blue-500 text-sm"
              placeholder="••••••••"
            />
          </div>

          <button
            type="submit"
            className="w-full py-3 bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-700 hover:to-indigo-700 text-white font-semibold rounded-xl text-sm shadow-lg shadow-blue-500/25 transition duration-150 active:scale-98"
          >
            Authenticate Admin
          </button>
        </form>

        <div className="pt-4 border-t border-slate-700/60 text-center">
          <p className="text-[11px] text-slate-500">
            Default Demo: <span className="text-blue-400 font-mono">admin@tapattend.com</span> / <span className="text-blue-400 font-mono">admin123</span>
          </p>
        </div>
      </div>
    </div>
  );
}

function DashboardView({ employees, attendance, departments, settings, onNavigate }) {
  const todayStr = getFormattedDate(0);

  // Compute stats
  const todayRecords = useMemo(() => {
    return attendance.filter(r => r.date === todayStr);
  }, [attendance, todayStr]);

  const totalEmployees = employees.length;
  const presentCount = todayRecords.length;
  const absentCount = Math.max(0, totalEmployees - presentCount);
  const totalDepts = departments.length;
  const attendancePercentage = totalEmployees > 0 ? Math.round((presentCount / totalEmployees) * 100) : 0;

  return (
    <div className="space-y-6">
      
      {/* Top Banner */}
      <div className="relative overflow-hidden rounded-2xl bg-gradient-to-r from-blue-800 via-blue-700 to-indigo-800 text-white p-6 shadow-xl">
        <div className="relative z-10 flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
          <div>
            <span className="inline-block px-2.5 py-1 bg-blue-500/30 rounded-md text-xs font-semibold tracking-wider uppercase mb-2">
              System Dashboard
            </span>
            <h2 className="text-2xl md:text-3xl font-extrabold tracking-tight">
              Real-Time NFC & Barcode Monitor
            </h2>
            <p className="text-blue-100 text-sm mt-1 max-w-xl">
              Strict standalone attendance tracking. Real-time logging active with zero payroll bloat.
            </p>
          </div>

          <button
            onClick={() => onNavigate('scanner')}
            className="px-5 py-3 bg-white text-blue-900 hover:bg-blue-50 font-bold text-sm rounded-xl shadow-lg transition active:scale-95 flex items-center space-x-2"
          >
            <svg className="w-5 h-5 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z" />
            </svg>
            <span>Open Attendance Scanner</span>
          </button>
        </div>
      </div>

      {/* Metric Cards Grid */}
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <StatCard
          title="Total Employees"
          value={totalEmployees}
          subtext="Registered roster"
          color="blue"
          icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />}
        />
        <StatCard
          title="Today's Attendance"
          value={presentCount}
          subtext={`Target shift: ${settings.startTime}`}
          color="emerald"
          icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />}
        />
        <StatCard
          title="Absent Employees"
          value={absentCount}
          subtext="Pending check-in"
          color="rose"
          icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />}
        />
        <StatCard
          title="Total Departments"
          value={totalDepts}
          subtext="Active divisions"
          color="indigo"
          icon={<path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5m0 0h5m-5 0V11m0 0h-5m5 0h5" />}
        />
      </div>

      {/* Charts & Status Section */}
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        {/* Progress Gauge */}
        <div className="bg-white dark:bg-slate-900 p-6 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm flex flex-col justify-between">
          <div>
            <h3 className="font-bold text-slate-800 dark:text-slate-100 text-base">Today's Attendance Turnout</h3>
            <p className="text-xs text-slate-500">Live ratio against employee roster</p>
          </div>

          <div className="my-6 flex flex-col items-center justify-center">
            {/* SVG Circular Gauge */}
            <div className="relative w-40 h-40 flex items-center justify-center">
              <svg className="w-full h-full transform -rotate-90" viewBox="0 0 36 36">
                <path
                  className="text-slate-200 dark:text-slate-800"
                  strokeWidth="3.5"
                  stroke="currentColor"
                  fill="none"
                  d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"
                />
                <path
                  className="text-blue-600 transition-all duration-1000 stroke-current"
                  strokeDasharray={`${attendancePercentage}, 100`}
                  strokeWidth="3.5"
                  strokeLinecap="round"
                  fill="none"
                  d="M18 2.0845 a 15.9155 15.9155 0 0 1 0 31.831 a 15.9155 15.9155 0 0 1 0 -31.831"
                />
              </svg>
              <div className="absolute text-center">
                <span className="text-3xl font-black text-slate-800 dark:text-slate-100">{attendancePercentage}%</span>
                <span className="block text-[10px] text-slate-400 uppercase font-semibold">Turnout Rate</span>
              </div>
            </div>
          </div>

          <div className="grid grid-cols-2 gap-2 text-center pt-3 border-t border-slate-100 dark:border-slate-800">
            <div>
              <p className="text-xs text-slate-400">Present</p>
              <p className="font-bold text-emerald-600 dark:text-emerald-400">{presentCount} Staff</p>
            </div>
            <div>
              <p className="text-xs text-slate-400">Absent</p>
              <p className="font-bold text-rose-500 dark:text-rose-400">{absentCount} Staff</p>
            </div>
          </div>
        </div>

        {/* Latest Scans Stream */}
        <div className="lg:col-span-2 bg-white dark:bg-slate-900 p-6 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm flex flex-col">
          <div className="flex items-center justify-between mb-4">
            <div>
              <h3 className="font-bold text-slate-800 dark:text-slate-100 text-base">Latest Attendance Feeds</h3>
              <p className="text-xs text-slate-500">Real-time scan logs from NFC & Barcode terminals</p>
            </div>
            <button
              onClick={() => onNavigate('records')}
              className="text-xs font-semibold text-blue-600 dark:text-blue-400 hover:underline"
            >
              View All Logs &rarr;
            </button>
          </div>

          <div className="overflow-x-auto flex-1">
            <table className="w-full text-left text-xs">
              <thead>
                <tr className="border-b border-slate-200 dark:border-slate-800 text-slate-400 font-semibold uppercase tracking-wider">
                  <th className="pb-3">Employee</th>
                  <th className="pb-3">Department</th>
                  <th className="pb-3">Time</th>
                  <th className="pb-3">Method</th>
                  <th className="pb-3">Status</th>
                </tr>
              </thead>
              <tbody className="divide-y divide-slate-100 dark:divide-slate-800">
                {attendance.slice(0, 5).map((rec) => (
                  <tr key={rec.id} className="hover:bg-slate-50 dark:hover:bg-slate-800/50 transition">
                    <td className="py-3 font-medium text-slate-900 dark:text-slate-100 flex items-center space-x-2">
                      <div className="w-7 h-7 rounded-full bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-200 flex items-center justify-center font-bold text-[10px]">
                        {rec.fullName.substring(0, 2)}
                      </div>
                      <div>
                        <p className="font-semibold">{rec.fullName}</p>
                        <p className="text-[10px] text-slate-400">{rec.employeeId}</p>
                      </div>
                    </td>
                    <td className="py-3 text-slate-600 dark:text-slate-300">{rec.department}</td>
                    <td className="py-3 font-mono font-medium text-slate-700 dark:text-slate-300">{rec.time}</td>
                    <td className="py-3">
                      <span className={`px-2 py-0.5 rounded-md text-[10px] font-bold ${
                        rec.method === 'NFC' 
                          ? 'bg-blue-100 text-blue-800 dark:bg-blue-900/60 dark:text-blue-300' 
                          : 'bg-purple-100 text-purple-800 dark:bg-purple-900/60 dark:text-purple-300'
                      }`}>
                        {rec.method}
                      </span>
                    </td>
                    <td className="py-3">
                      <span className={`px-2 py-0.5 rounded-full text-[10px] font-semibold ${
                        rec.status === 'On Time' ? 'bg-emerald-100 text-emerald-800 dark:bg-emerald-900/60 dark:text-emerald-300' : 'bg-amber-100 text-amber-800 dark:bg-amber-900/60 dark:text-amber-300'
                      }`}>
                        {rec.status}
                      </span>
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  );
}

function StatCard({ title, value, subtext, icon, color }) {
  const colorStyles = {
    blue: 'bg-blue-50 text-blue-600 dark:bg-blue-950/40 dark:text-blue-400 border-blue-100 dark:border-blue-900/50',
    emerald: 'bg-emerald-50 text-emerald-600 dark:bg-emerald-950/40 dark:text-emerald-400 border-emerald-100 dark:border-emerald-900/50',
    rose: 'bg-rose-50 text-rose-600 dark:bg-rose-950/40 dark:text-rose-400 border-rose-100 dark:border-rose-900/50',
    indigo: 'bg-indigo-50 text-indigo-600 dark:bg-indigo-950/40 dark:text-indigo-400 border-indigo-100 dark:border-indigo-900/50'
  };

  return (
    <div className={`p-5 rounded-2xl bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 shadow-sm flex items-center justify-between`}>
      <div>
        <p className="text-xs font-semibold text-slate-500 dark:text-slate-400 uppercase tracking-wider">{title}</p>
        <h4 className="text-3xl font-extrabold text-slate-900 dark:text-white mt-1 tracking-tight">{value}</h4>
        <p className="text-[11px] text-slate-400 mt-1">{subtext}</p>
      </div>
      <div className={`w-12 h-12 rounded-xl border flex items-center justify-center ${colorStyles[color]}`}>
        <svg className="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          {icon}
        </svg>
      </div>
    </div>
  );
}

function ScannerView({ employees, attendance, setAttendance, settings, showToast }) {
  const [manualCode, setManualCode] = useState('');
  const [lastScannedResult, setLastScannedResult] = useState(null);
  const [duplicateWarning, setDuplicateWarning] = useState(null);
  const [isScanningActive, setIsScanningActive] = useState(true);
  const [isBarcodeModalOpen, setIsBarcodeModalOpen] = useState(false);

  const resetTimerRef = useRef(null);

  // Auto clear result banner after 3 seconds
  const triggerSuccessDisplay = (resultObj) => {
    setLastScannedResult(resultObj);
    setDuplicateWarning(null);
    if (settings.playSoundFeedback) playAudioFeedback('success');

    if (resetTimerRef.current) clearTimeout(resetTimerRef.current);
    resetTimerRef.current = setTimeout(() => {
      setLastScannedResult(null);
    }, 3000);
  };

  const triggerDuplicateDisplay = (employeeName) => {
    setDuplicateWarning(`Attendance already recorded today for ${employeeName}.`);
    setLastScannedResult(null);
    if (settings.playSoundFeedback) playAudioFeedback('duplicate');

    if (resetTimerRef.current) clearTimeout(resetTimerRef.current);
    resetTimerRef.current = setTimeout(() => {
      setDuplicateWarning(null);
    }, 4000);
  };

  // Core scan handler (works for both NFC UID and Barcode)
  const processAttendanceScan = (identifier, method = 'NFC') => {
    if (!identifier) return;

    // Search employee by NFC UID or Barcode or Employee ID
    const cleanId = identifier.trim().toUpperCase();
    const foundEmp = employees.find(
      e => e.nfcUid.toUpperCase() === cleanId || 
           e.barcode.toUpperCase() === cleanId || 
           e.id.toUpperCase() === cleanId
    );

    if (!foundEmp) {
      showToast(`No employee matched identifier: "${identifier}"`, 'error');
      return;
    }

    // Check duplicate for today
    const todayStr = getFormattedDate(0);
    const existingLog = attendance.find(
      a => a.employeeId === foundEmp.id && a.date === todayStr
    );

    if (existingLog) {
      triggerDuplicateDisplay(foundEmp.fullName);
      return;
    }

    // Record attendance
    const now = new Date();
    const timeStr = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    
    // Check if late based on settings.startTime
    const [startH, startM] = settings.startTime.split(':').map(Number);
    const isLate = (now.getHours() > startH) || (now.getHours() === startH && now.getMinutes() > startM);

    const newRecord = {
      id: `ATT-${Math.floor(1000 + Math.random() * 9000)}`,
      employeeId: foundEmp.id,
      fullName: foundEmp.fullName,
      department: foundEmp.department,
      phone: foundEmp.phone,
      date: todayStr,
      time: timeStr,
      method: method,
      status: isLate ? 'Late' : 'On Time',
      timestamp: Date.now()
    };

    setAttendance(prev => [newRecord, ...prev]);

    triggerSuccessDisplay({
      employee: foundEmp,
      record: newRecord
    });
  };

  // Web NFC Web API Scanner integration (if hardware available)
  useEffect(() => {
    let ndef;
    const initNFC = async () => {
      if ('NDEFReader' in window && isScanningActive) {
        try {
          ndef = new window.NDEFReader();
          await ndef.scan();
          ndef.addEventListener("reading", ({ serialNumber }) => {
            if (serialNumber) {
              processAttendanceScan(serialNumber, 'NFC');
            }
          });
        } catch (error) {
          console.warn("Web NFC API setup failed or unsupported:", error);
        }
      }
    };
    initNFC();
  }, [isScanningActive, employees, attendance]);

  return (
    <div className="max-w-4xl mx-auto space-y-6">
      
      {/* Kiosk Mode Barcode Modal */}
      {isBarcodeModalOpen && (
        <div className="fixed inset-0 z-50 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4">
          <div className="bg-white dark:bg-slate-900 rounded-2xl max-w-md w-full p-6 border border-slate-200 dark:border-slate-800 shadow-2xl space-y-4">
            <div className="flex items-center justify-between border-b pb-3 dark:border-slate-800">
              <h3 className="font-bold text-slate-800 dark:text-white text-lg">Barcode / QR Reader Backup</h3>
              <button 
                onClick={() => setIsBarcodeModalOpen(false)}
                className="text-slate-400 hover:text-slate-600 dark:hover:text-slate-200"
              >
                ✕
              </button>
            </div>
            
            <p className="text-xs text-slate-500">
              Scan barcode using hand scanner or type the Barcode ID manually below.
            </p>

            <div>
              <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 uppercase tracking-wider mb-1">
                Barcode Number
              </label>
              <input
                type="text"
                autoFocus
                placeholder="e.g. TAP-1001-88"
                value={manualCode}
                onChange={(e) => setManualCode(e.target.value)}
                onKeyDown={(e) => {
                  if (e.key === 'Enter') {
                    processAttendanceScan(manualCode, 'Barcode');
                    setManualCode('');
                    setIsBarcodeModalOpen(false);
                  }
                }}
                className="w-full px-4 py-3 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-slate-900 dark:text-white font-mono text-sm focus:ring-2 focus:ring-blue-500 outline-none"
              />
            </div>

            <div className="flex justify-end space-x-2 pt-2">
              <button
                onClick={() => setIsBarcodeModalOpen(false)}
                className="px-4 py-2 rounded-xl text-xs font-semibold text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800"
              >
                Cancel
              </button>
              <button
                onClick={() => {
                  processAttendanceScan(manualCode, 'Barcode');
                  setManualCode('');
                  setIsBarcodeModalOpen(false);
                }}
                className="px-4 py-2 rounded-xl text-xs font-semibold bg-blue-600 hover:bg-blue-700 text-white shadow-md"
              >
                Submit Barcode Scan
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Main Terminal Box */}
      <div className="bg-white dark:bg-slate-900 rounded-3xl border border-slate-200 dark:border-slate-800 shadow-xl overflow-hidden">
        
        {/* Header Terminal Status */}
        <div className="bg-slate-900 text-white px-6 py-4 flex items-center justify-between">
          <div className="flex items-center space-x-3">
            <span className="relative flex h-3 w-3">
              <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span>
              <span className="relative inline-flex rounded-full h-3 w-3 bg-emerald-500"></span>
            </span>
            <span className="font-mono text-xs uppercase tracking-wider text-slate-300">Terminal #01 Active • NFC Ready</span>
          </div>

          <button
            onClick={() => setIsBarcodeModalOpen(true)}
            className="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 rounded-lg text-xs font-medium text-slate-200 flex items-center space-x-1.5 transition"
          >
            <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z" />
            </svg>
            <span>Barcode Backup Scan</span>
          </button>
        </div>

        {/* Display Area */}
        <div className="p-8 flex flex-col items-center justify-center min-h-[380px]">
          
          {/* SUCCESS OVERLAY (3 seconds duration) */}
          {lastScannedResult ? (
            <div className="w-full max-w-lg bg-emerald-500/10 border-2 border-emerald-500 rounded-2xl p-6 text-center animate-fade-in space-y-4 shadow-lg">
              <div className="w-16 h-16 bg-emerald-500 text-white rounded-full flex items-center justify-center mx-auto shadow-md animate-bounce">
                <svg className="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="3" d="M5 13l4 4L19 7" />
                </svg>
              </div>

              <div>
                <span className="px-3 py-1 bg-emerald-500 text-white rounded-full text-xs font-bold uppercase tracking-wider">
                  Attendance Recorded
                </span>
                <h3 className="text-2xl font-black text-slate-900 dark:text-white mt-2">
                  {lastScannedResult.employee.fullName}
                </h3>
                <p className="text-sm font-semibold text-slate-600 dark:text-slate-300">
                  {lastScannedResult.employee.department} • {lastScannedResult.employee.id}
                </p>
              </div>

              <div className="grid grid-cols-3 gap-2 py-3 bg-white dark:bg-slate-800 rounded-xl text-xs font-mono border dark:border-slate-700">
                <div>
                  <p className="text-[10px] text-slate-400">Date</p>
                  <p className="font-bold text-slate-800 dark:text-slate-200">{lastScannedResult.record.date}</p>
                </div>
                <div>
                  <p className="text-[10px] text-slate-400">Time</p>
                  <p className="font-bold text-slate-800 dark:text-slate-200">{lastScannedResult.record.time}</p>
                </div>
                <div>
                  <p className="text-[10px] text-slate-400">Method</p>
                  <p className="font-bold text-emerald-600 dark:text-emerald-400">{lastScannedResult.record.method}</p>
                </div>
              </div>

              <p className="text-[11px] text-slate-400 animate-pulse">
                Returning to reader mode in 3 seconds...
              </p>
            </div>
          ) : duplicateWarning ? (
            /* DUPLICATE WARNING OVERLAY */
            <div className="w-full max-w-lg bg-amber-500/10 border-2 border-amber-500 rounded-2xl p-6 text-center animate-fade-in space-y-4">
              <div className="w-16 h-16 bg-amber-500 text-white rounded-full flex items-center justify-center mx-auto shadow-md">
                <svg className="w-10 h-10" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
                </svg>
              </div>
              <h3 className="text-xl font-bold text-amber-600 dark:text-amber-400">Duplicate Tap Detected</h3>
              <p className="text-sm font-semibold text-slate-700 dark:text-slate-200">{duplicateWarning}</p>
              <p className="text-[11px] text-slate-400">System prevents multiple daily check-ins for the same staff member.</p>
            </div>
          ) : (
            /* DEFAULT WAITING STATE */
            <div className="text-center space-y-6">
              
              {/* NFC Sensor Pulse Animation */}
              <div className="relative flex items-center justify-center">
                <div className="absolute w-44 h-44 rounded-full bg-blue-500/10 animate-ping" />
                <div className="absolute w-36 h-36 rounded-full bg-blue-500/20" />
                <div className="relative w-28 h-28 rounded-full bg-gradient-to-tr from-blue-600 to-indigo-600 text-white flex items-center justify-center shadow-2xl shadow-blue-500/40">
                  <svg className="w-14 h-14" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 11c0 3.517-1.009 6.799-2.753 9.571m-3.44-2.04l.054-.09A13.916 13.916 0 008 11a4 4 0 118 0c0 1.017-.07 2.019-.203 3m-2.118 6.844A21.88 21.88 0 0015.171 17m3.839 1.132c.645-2.266.99-4.659.99-7.132A8 8 0 008 4.07M3 15.364c.64-1.319 1-2.8 1-4.364 0-1.457-.39-2.823-1.07-4" />
                  </svg>
                </div>
              </div>

              <div>
                <h2 className="text-3xl font-black text-slate-900 dark:text-white tracking-tight">
                  Ready to Scan
                </h2>
                <p className="text-slate-500 text-sm mt-1">
                  Hold staff NFC card against reader or click a card below to test
                </p>
              </div>
            </div>
          )}

        </div>

        {/* Quick NFC Card Tap Simulator (For Testing/Demo Purpose) */}
        <div className="bg-slate-50 dark:bg-slate-800/60 p-6 border-t border-slate-200 dark:border-slate-800">
          <div className="flex items-center justify-between mb-3">
            <span className="text-xs font-bold text-slate-500 uppercase tracking-wider">
              NFC Card Simulator (Click employee card to trigger tap)
            </span>
            <span className="text-[10px] bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300 px-2 py-0.5 rounded font-mono">
              Hardware Tester
            </span>
          </div>

          <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-6 gap-2">
            {employees.map((emp) => (
              <button
                key={emp.id}
                onClick={() => processAttendanceScan(emp.nfcUid, 'NFC')}
                className="p-3 bg-white dark:bg-slate-900 hover:border-blue-500 border border-slate-200 dark:border-slate-700 rounded-xl text-left transition shadow-sm group hover:-translate-y-0.5 active:translate-y-0"
              >
                <div className="flex items-center space-x-2">
                  <div className="w-6 h-6 rounded-full bg-slate-100 dark:bg-slate-800 text-slate-700 dark:text-slate-300 font-bold text-[9px] flex items-center justify-center">
                    {emp.fullName.substring(0, 2)}
                  </div>
                  <div className="min-w-0 flex-1">
                    <p className="text-xs font-bold text-slate-800 dark:text-slate-200 truncate group-hover:text-blue-600 dark:group-hover:text-blue-400">
                      {emp.fullName.split(' ')[0]}
                    </p>
                    <p className="text-[9px] text-slate-400 font-mono truncate">{emp.nfcUid}</p>
                  </div>
                </div>
              </button>
            ))}
          </div>
        </div>

      </div>
    </div>
  );
}

function AttendanceRecordsView({ attendance, employees, departments, settings }) {
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedDeptFilter, setSelectedDeptFilter] = useState('ALL');
  const [dateRangeFilter, setDateRangeFilter] = useState('TODAY');
  const [customDate, setCustomDate] = useState(getFormattedDate(0));

  const todayStr = getFormattedDate(0);
  const yesterdayStr = getFormattedDate(1);

  // Filtered dataset
  const filteredRecords = useMemo(() => {
    return attendance.filter(rec => {
      // Search matches
      const matchesSearch = 
        rec.fullName.toLowerCase().includes(searchTerm.toLowerCase()) ||
        rec.employeeId.toLowerCase().includes(searchTerm.toLowerCase()) ||
        rec.department.toLowerCase().includes(searchTerm.toLowerCase());

      // Department filter
      const matchesDept = selectedDeptFilter === 'ALL' || rec.department === selectedDeptFilter;

      // Date range filter
      let matchesDate = true;
      if (dateRangeFilter === 'TODAY') {
        matchesDate = rec.date === todayStr;
      } else if (dateRangeFilter === 'YESTERDAY') {
        matchesDate = rec.date === yesterdayStr;
      } else if (dateRangeFilter === 'CUSTOM') {
        matchesDate = rec.date === customDate;
      }

      return matchesSearch && matchesDept && matchesDate;
    });
  }, [attendance, searchTerm, selectedDeptFilter, dateRangeFilter, customDate, todayStr, yesterdayStr]);

  // Download CSV Export
  const exportToCSV = () => {
    const headers = ['Attendance ID,Employee ID,Full Name,Department,Phone Number,Date,Time,Method,Status\n'];
    const rows = filteredRecords.map(r => 
      `"${r.id}","${r.employeeId}","${r.fullName}","${r.department}","${r.phone}","${r.date}","${r.time}","${r.method}","${r.status}"`
    );
    const blob = new Blob([headers.concat(rows.join('\n'))], { type: 'text/csv' });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `TapAttend_Report_${dateRangeFilter}_${new Date().toISOString().slice(0,10)}.csv`;
    a.click();
  };

  // Generate & Print formatted HTML/PDF Report
  const triggerPrintPDF = () => {
    const printWindow = window.open('', '_blank');
    const htmlContent = `
      <!DOCTYPE html>
      <html>
      <head>
        <title>Daily Attendance Report - ${settings.orgName}</title>
        <style>
          body { font-family: 'Segoe UI', Arial, sans-serif; padding: 40px; color: #1e293b; }
          .header { display: flex; justify-content: space-between; align-items: center; border-b: 2px solid #2563eb; padding-bottom: 20px; margin-bottom: 30px; }
          .logo-title { font-size: 24px; font-weight: 800; color: #1e40af; margin: 0; }
          .sub { font-size: 12px; color: #64748b; margin-top: 4px; }
          .meta { font-size: 12px; text-align: right; }
          table { width: 100%; border-collapse: collapse; margin-top: 20px; font-size: 12px; }
          th { background: #f1f5f9; text-align: left; padding: 10px; font-weight: 700; color: #475569; border-bottom: 2px solid #cbd5e1; }
          td { padding: 10px; border-bottom: 1px solid #e2e8f0; }
          .footer { margin-top: 40px; text-align: center; font-size: 11px; color: #94a3b8; border-t: 1px solid #e2e8f0; padding-top: 15px; }
          .badge { padding: 3px 8px; border-radius: 4px; font-weight: bold; font-size: 10px; }
          .badge-nfc { background: #dbeafe; color: #1e40af; }
          .badge-barcode { background: #f3e8ff; color: #6b21a8; }
        </style>
      </head>
      <body>
        <div class="header">
          <div>
            <h1 class="logo-title">ExSofós TapAttend</h1>
            <p class="sub">${settings.orgName} • Daily Attendance Record</p>
          </div>
          <div class="meta">
            <p><strong>Date Generated:</strong> ${new Date().toLocaleDateString()}</p>
            <p><strong>Filter Scope:</strong> ${dateRangeFilter}</p>
            <p><strong>Total Logs:</strong> ${filteredRecords.length}</p>
          </div>
        </div>

        <table>
          <thead>
            <tr>
              <th>ID</th>
              <th>Full Name</th>
              <th>Department</th>
              <th>Phone Number</th>
              <th>Date</th>
              <th>Time</th>
              <th>Method</th>
              <th>Status</th>
            </tr>
          </thead>
          <tbody>
            ${filteredRecords.map(r => `
              <tr>
                <td><strong>${r.employeeId}</strong></td>
                <td>${r.fullName}</td>
                <td>${r.department}</td>
                <td>${r.phone}</td>
                <td>${r.date}</td>
                <td>${r.time}</td>
                <td><span class="badge ${r.method === 'NFC' ? 'badge-nfc' : 'badge-barcode'}">${r.method}</span></td>
                <td>${r.status}</td>
              </tr>
            `).join('')}
          </tbody>
        </table>

        <div class="footer">
          Generated Automatically by ExSofós TapAttend Management Platform • Official Records
        </div>

        <script>
          window.onload = function() { window.print(); }
        </script>
      </body>
      </html>
    `;
    printWindow.document.write(htmlContent);
    printWindow.document.close();
  };

  return (
    <div className="space-y-6">
      
      {/* Header Controls & Actions */}
      <div className="bg-white dark:bg-slate-900 p-6 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm space-y-4">
        
        <div className="flex flex-col md:flex-row md:items-center justify-between gap-4">
          <div>
            <h3 className="font-bold text-slate-900 dark:text-white text-lg">Attendance Logs Table</h3>
            <p className="text-xs text-slate-500">Filter and export staff scan records</p>
          </div>

          <div className="flex flex-wrap items-center gap-2">
            <button
              onClick={exportToCSV}
              className="px-3.5 py-2 rounded-xl bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 font-semibold text-xs flex items-center space-x-1.5 transition"
            >
              <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
              </svg>
              <span>Export CSV</span>
            </button>

            <button
              onClick={triggerPrintPDF}
              className="px-3.5 py-2 rounded-xl bg-blue-600 hover:bg-blue-700 text-white font-semibold text-xs flex items-center space-x-1.5 shadow-md shadow-blue-500/20 transition"
            >
              <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M17 17h2a2 2 0 002-2v-4a2 2 0 00-2-2H5a2 2 0 00-2 2v4a2 2 0 002 2h2m2 4h6a2 2 0 002-2v-4a2 2 0 00-2-2H9a2 2 0 00-2 2v4a2 2 0 002 2zm8-12V5a2 2 0 00-2-2H9a2 2 0 00-2 2v4h10z" />
              </svg>
              <span>Print PDF Report</span>
            </button>
          </div>
        </div>

        {/* Search & Filter Controls */}
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-3 pt-2">
          
          {/* Search Bar */}
          <div>
            <input
              type="text"
              placeholder="Search Name or Employee ID..."
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-slate-800 dark:text-slate-100 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
            />
          </div>

          {/* Department Filter */}
          <div>
            <select
              value={selectedDeptFilter}
              onChange={(e) => setSelectedDeptFilter(e.target.value)}
              className="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-slate-800 dark:text-slate-100 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
            >
              <option value="ALL">All Departments</option>
              {departments.map(d => (
                <option key={d.id} value={d.name}>{d.name}</option>
              ))}
            </select>
          </div>

          {/* Date Filter */}
          <div>
            <select
              value={dateRangeFilter}
              onChange={(e) => setDateRangeFilter(e.target.value)}
              className="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-slate-800 dark:text-slate-100 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
            >
              <option value="TODAY">Today ({todayStr})</option>
              <option value="YESTERDAY">Yesterday ({yesterdayStr})</option>
              <option value="ALL">All Recorded Dates</option>
              <option value="CUSTOM">Custom Date</option>
            </select>
          </div>

          {/* Custom Date Picker */}
          {dateRangeFilter === 'CUSTOM' && (
            <div>
              <input
                type="date"
                value={customDate}
                onChange={(e) => setCustomDate(e.target.value)}
                className="w-full px-3.5 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-200 dark:border-slate-700 text-slate-800 dark:text-slate-100 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
              />
            </div>
          )}

        </div>
      </div>

      {/* Main Table */}
      <div className="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm overflow-hidden">
        <div className="overflow-x-auto">
          <table className="w-full text-left text-xs">
            <thead>
              <tr className="bg-slate-50 dark:bg-slate-800/70 border-b border-slate-200 dark:border-slate-800 text-slate-500 font-bold uppercase tracking-wider">
                <th className="p-4">Employee ID</th>
                <th className="p-4">Full Name</th>
                <th className="p-4">Department</th>
                <th className="p-4">Phone Number</th>
                <th className="p-4">Date</th>
                <th className="p-4">Time</th>
                <th className="p-4">Method</th>
                <th className="p-4">Status</th>
              </tr>
            </thead>
            <tbody className="divide-y divide-slate-100 dark:divide-slate-800">
              {filteredRecords.length > 0 ? (
                filteredRecords.map((rec) => (
                  <tr key={rec.id} className="hover:bg-slate-50 dark:hover:bg-slate-800/50 transition">
                    <td className="p-4 font-mono font-bold text-slate-900 dark:text-slate-100">{rec.employeeId}</td>
                    <td className="p-4 font-semibold text-slate-800 dark:text-slate-200">{rec.fullName}</td>
                    <td className="p-4 text-slate-600 dark:text-slate-400">{rec.department}</td>
                    <td className="p-4 font-mono text-slate-500">{rec.phone}</td>
                    <td className="p-4 font-mono text-slate-700 dark:text-slate-300">{rec.date}</td>
                    <td className="p-4 font-mono font-bold text-slate-800 dark:text-slate-200">{rec.time}</td>
                    <td className="p-4">
                      <span className={`px-2 py-0.5 rounded-md text-[10px] font-bold ${
                        rec.method === 'NFC' 
                          ? 'bg-blue-100 text-blue-800 dark:bg-blue-900/60 dark:text-blue-300' 
                          : 'bg-purple-100 text-purple-800 dark:bg-purple-900/60 dark:text-purple-300'
                      }`}>
                        {rec.method}
                      </span>
                    </td>
                    <td className="p-4">
                      <span className={`px-2 py-0.5 rounded-full text-[10px] font-semibold ${
                        rec.status === 'On Time' ? 'bg-emerald-100 text-emerald-800 dark:bg-emerald-900/60 dark:text-emerald-300' : 'bg-amber-100 text-amber-800 dark:bg-amber-900/60 dark:text-amber-300'
                      }`}>
                        {rec.status}
                      </span>
                    </td>
                  </tr>
                ))
              ) : (
                <tr>
                  <td colSpan={8} className="p-8 text-center text-slate-400">
                    No attendance records match your search query.
                  </td>
                </tr>
              )}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  );
}

function DepartmentsView({ departments, setDepartments, employees, showToast }) {
  const [isAddModalOpen, setIsAddModalOpen] = useState(false);
  const [editingDept, setEditingDept] = useState(null);
  const [name, setName] = useState('');
  const [code, setCode] = useState('');
  const [description, setDescription] = useState('');

  const openAdd = () => {
    setEditingDept(null);
    setName('');
    setCode('');
    setDescription('');
    setIsAddModalOpen(true);
  };

  const openEdit = (dept) => {
    setEditingDept(dept);
    setName(dept.name);
    setCode(dept.code);
    setDescription(dept.description);
    setIsAddModalOpen(true);
  };

  const handleSave = (e) => {
    e.preventDefault();
    if (editingDept) {
      setDepartments(prev => prev.map(d => d.id === editingDept.id ? { ...d, name, code, description } : d));
      showToast('Department updated successfully', 'success');
    } else {
      const newDept = {
        id: `DEP-0${departments.length + 1}`,
        name,
        code: code.toUpperCase(),
        description
      };
      setDepartments(prev => [...prev, newDept]);
      showToast('New department added', 'success');
    }
    setIsAddModalOpen(false);
  };

  const handleDelete = (id) => {
    setDepartments(prev => prev.filter(d => d.id !== id));
    showToast('Department removed', 'info');
  };

  return (
    <div className="space-y-6">
      
      <div className="flex items-center justify-between">
        <div>
          <h3 className="font-bold text-slate-900 dark:text-white text-lg">Organization Departments</h3>
          <p className="text-xs text-slate-500">Configure organizational units and view staff allocation</p>
        </div>
        <button
          onClick={openAdd}
          className="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white rounded-xl text-xs font-semibold shadow-md shadow-blue-500/20 flex items-center space-x-1.5 transition"
        >
          <span>+ Add Department</span>
        </button>
      </div>

      {/* Department Modal */}
      {isAddModalOpen && (
        <div className="fixed inset-0 z-50 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4">
          <form onSubmit={handleSave} className="bg-white dark:bg-slate-900 rounded-2xl max-w-md w-full p-6 border border-slate-200 dark:border-slate-800 shadow-2xl space-y-4">
            <h3 className="font-bold text-slate-800 dark:text-white text-lg">
              {editingDept ? 'Edit Department' : 'Create Department'}
            </h3>
            
            <div>
              <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Department Name</label>
              <input
                type="text"
                required
                value={name}
                onChange={(e) => setName(e.target.value)}
                placeholder="e.g. Finance & Operations"
                className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-sm outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>

            <div>
              <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Short Code</label>
              <input
                type="text"
                required
                value={code}
                onChange={(e) => setCode(e.target.value)}
                placeholder="e.g. FIN"
                className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-sm uppercase font-mono outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>

            <div>
              <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Description</label>
              <textarea
                value={description}
                onChange={(e) => setDescription(e.target.value)}
                rows={3}
                placeholder="Brief role of department..."
                className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-sm outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>

            <div className="flex justify-end space-x-2 pt-2">
              <button
                type="button"
                onClick={() => setIsAddModalOpen(false)}
                className="px-4 py-2 rounded-xl text-xs font-semibold text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800"
              >
                Cancel
              </button>
              <button
                type="submit"
                className="px-4 py-2 rounded-xl text-xs font-semibold bg-blue-600 text-white shadow-md hover:bg-blue-700"
              >
                Save Department
              </button>
            </div>
          </form>
        </div>
      )}

      {/* Cards Grid */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        {departments.map((dept) => {
          const deptEmployees = employees.filter(e => e.department === dept.name);
          return (
            <div key={dept.id} className="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 p-5 shadow-sm flex flex-col justify-between space-y-4">
              <div>
                <div className="flex items-center justify-between">
                  <span className="px-2.5 py-1 bg-blue-100 text-blue-800 dark:bg-blue-900/60 dark:text-blue-300 font-mono font-bold text-xs rounded-lg">
                    {dept.code}
                  </span>
                  <div className="flex items-center space-x-1">
                    <button 
                      onClick={() => openEdit(dept)}
                      className="p-1 text-slate-400 hover:text-blue-600 transition"
                      title="Edit"
                    >
                      <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" />
                      </svg>
                    </button>
                    <button 
                      onClick={() => handleDelete(dept.id)}
                      className="p-1 text-slate-400 hover:text-rose-600 transition"
                      title="Delete"
                    >
                      <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                      </svg>
                    </button>
                  </div>
                </div>

                <h4 className="text-base font-bold text-slate-900 dark:text-white mt-3">{dept.name}</h4>
                <p className="text-xs text-slate-500 mt-1">{dept.description}</p>
              </div>

              <div className="pt-3 border-t border-slate-100 dark:border-slate-800 flex items-center justify-between text-xs">
                <span className="text-slate-400 font-medium">Assigned Staff</span>
                <span className="font-bold text-blue-600 dark:text-blue-400 bg-blue-50 dark:bg-blue-950 px-2 py-0.5 rounded-full">
                  {deptEmployees.length} Employees
                </span>
              </div>
            </div>
          );
        })}
      </div>

    </div>
  );
}

function EmployeesView({ employees, setEmployees, departments, showToast }) {
  const [isModalOpen, setIsModalOpen] = useState(false);
  const [editingEmp, setEditingEmp] = useState(null);
  const [activeBarcodeEmp, setActiveBarcodeEmp] = useState(null);

  // Form Fields
  const [fullName, setFullName] = useState('');
  const [empId, setEmpId] = useState('');
  const [phone, setPhone] = useState('');
  const [department, setDepartment] = useState(departments[0]?.name || '');
  const [nfcUid, setNfcUid] = useState('');
  const [barcode, setBarcode] = useState('');

  const openAddModal = () => {
    setEditingEmp(null);
    setFullName('');
    setEmpId(`EMP-${1000 + employees.length + 1}`);
    setPhone('+1 (555) 000-0000');
    setDepartment(departments[0]?.name || '');
    setNfcUid(`04:${Math.floor(10+Math.random()*89)}:${Math.floor(10+Math.random()*89)}:${Math.floor(10+Math.random()*89)}`);
    setBarcode(`TAP-${1000 + employees.length + 1}-99`);
    setIsModalOpen(true);
  };

  const openEditModal = (emp) => {
    setEditingEmp(emp);
    setFullName(emp.fullName);
    setEmpId(emp.id);
    setPhone(emp.phone);
    setDepartment(emp.department);
    setNfcUid(emp.nfcUid);
    setBarcode(emp.barcode);
    setIsModalOpen(true);
  };

  const handleSave = (e) => {
    e.preventDefault();
    if (editingEmp) {
      setEmployees(prev => prev.map(e => e.id === editingEmp.id ? {
        ...e, fullName, phone, department, nfcUid, barcode
      } : e));
      showToast('Employee details updated', 'success');
    } else {
      const newEmployee = {
        id: empId,
        fullName,
        department,
        phone,
        nfcUid,
        barcode,
        status: 'Active',
        avatar: `https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?w=150&auto=format&fit=crop&q=80`
      };
      setEmployees(prev => [...prev, newEmployee]);
      showToast('New employee registered', 'success');
    }
    setIsModalOpen(false);
  };

  const handleDelete = (id) => {
    setEmployees(prev => prev.filter(e => e.id !== id));
    showToast('Employee removed from roster', 'info');
  };

  return (
    <div className="space-y-6">
      
      {/* Top Header */}
      <div className="flex items-center justify-between">
        <div>
          <h3 className="font-bold text-slate-900 dark:text-white text-lg">Employee Credentials & Roster</h3>
          <p className="text-xs text-slate-500">Manage NFC tags and Barcode identifiers for staff</p>
        </div>
        <button
          onClick={openAddModal}
          className="px-4 py-2 bg-blue-600 hover:bg-blue-700 text-white rounded-xl text-xs font-semibold shadow-md shadow-blue-500/20 flex items-center space-x-1.5 transition"
        >
          <span>+ Add Employee</span>
        </button>
      </div>

      {/* Barcode Preview Modal */}
      {activeBarcodeEmp && (
        <div className="fixed inset-0 z-50 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4">
          <div className="bg-white dark:bg-slate-900 rounded-2xl max-w-sm w-full p-6 border border-slate-200 dark:border-slate-800 shadow-2xl text-center space-y-4">
            <h3 className="font-bold text-slate-800 dark:text-white text-base">Digital ID & Barcode Badge</h3>
            <div className="p-4 bg-slate-50 dark:bg-slate-800 rounded-xl border dark:border-slate-700 space-y-2">
              <p className="font-extrabold text-slate-900 dark:text-white text-sm">{activeBarcodeEmp.fullName}</p>
              <p className="text-xs text-slate-500">{activeBarcodeEmp.department} • {activeBarcodeEmp.id}</p>
              
              {/* Simulated SVG Barcode Graphic */}
              <div className="pt-3 pb-1 flex flex-col items-center">
                <svg className="w-48 h-16" viewBox="0 0 200 60">
                  <rect width="200" height="60" fill="white" />
                  <g fill="black">
                    <rect x="10" y="5" width="4" height="40" />
                    <rect x="18" y="5" width="2" height="40" />
                    <rect x="24" y="5" width="6" height="40" />
                    <rect x="34" y="5" width="2" height="40" />
                    <rect x="40" y="5" width="8" height="40" />
                    <rect x="52" y="5" width="4" height="40" />
                    <rect x="60" y="5" width="2" height="40" />
                    <rect x="68" y="5" width="6" height="40" />
                    <rect x="78" y="5" width="4" height="40" />
                    <rect x="88" y="5" width="2" height="40" />
                    <rect x="94" y="5" width="8" height="40" />
                    <rect x="106" y="5" width="4" height="40" />
                    <rect x="114" y="5" width="2" height="40" />
                    <rect x="122" y="5" width="6" height="40" />
                    <rect x="134" y="5" width="4" height="40" />
                    <rect x="144" y="5" width="8" height="40" />
                    <rect x="156" y="5" width="2" height="40" />
                    <rect x="162" y="5" width="6" height="40" />
                    <rect x="174" y="5" width="4" height="40" />
                    <rect x="182" y="5" width="6" height="40" />
                  </g>
                </svg>
                <p className="font-mono text-xs font-bold text-slate-800 tracking-widest mt-1">
                  {activeBarcodeEmp.barcode}
                </p>
              </div>
            </div>

            <button
              onClick={() => setActiveBarcodeEmp(null)}
              className="w-full py-2 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 dark:hover:bg-slate-700 text-slate-700 dark:text-slate-200 text-xs font-semibold rounded-xl"
            >
              Close Badge Preview
            </button>
          </div>
        </div>
      )}

      {/* Add / Edit Employee Form Modal */}
      {isModalOpen && (
        <div className="fixed inset-0 z-50 bg-slate-900/80 backdrop-blur-sm flex items-center justify-center p-4">
          <form onSubmit={handleSave} className="bg-white dark:bg-slate-900 rounded-2xl max-w-lg w-full p-6 border border-slate-200 dark:border-slate-800 shadow-2xl space-y-4">
            <h3 className="font-bold text-slate-800 dark:text-white text-lg border-b pb-3 dark:border-slate-800">
              {editingEmp ? 'Edit Employee Credentials' : 'Register New Employee'}
            </h3>

            <div className="grid grid-cols-2 gap-3">
              <div>
                <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Full Name</label>
                <input
                  type="text"
                  required
                  value={fullName}
                  onChange={(e) => setFullName(e.target.value)}
                  placeholder="John Doe"
                  className="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
                />
              </div>
              <div>
                <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Employee ID</label>
                <input
                  type="text"
                  required
                  readOnly={!!editingEmp}
                  value={empId}
                  onChange={(e) => setEmpId(e.target.value)}
                  className="w-full px-3 py-2 rounded-xl bg-slate-100 dark:bg-slate-800/60 border border-slate-300 dark:border-slate-700 text-xs font-mono font-bold outline-none"
                />
              </div>
            </div>

            <div className="grid grid-cols-2 gap-3">
              <div>
                <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Department</label>
                <select
                  value={department}
                  onChange={(e) => setDepartment(e.target.value)}
                  className="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
                >
                  {departments.map(d => (
                    <option key={d.id} value={d.name}>{d.name}</option>
                  ))}
                </select>
              </div>
              <div>
                <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Phone Number</label>
                <input
                  type="text"
                  value={phone}
                  onChange={(e) => setPhone(e.target.value)}
                  className="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-xs focus:ring-2 focus:ring-blue-500 outline-none"
                />
              </div>
            </div>

            <div className="grid grid-cols-2 gap-3 pt-2 border-t dark:border-slate-800">
              <div>
                <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">NFC Card UID</label>
                <input
                  type="text"
                  required
                  value={nfcUid}
                  onChange={(e) => setNfcUid(e.target.value)}
                  placeholder="04:A2:89:FE:32"
                  className="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 font-mono text-xs focus:ring-2 focus:ring-blue-500 outline-none"
                />
              </div>
              <div>
                <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Barcode Serial</label>
                <input
                  type="text"
                  required
                  value={barcode}
                  onChange={(e) => setBarcode(e.target.value)}
                  placeholder="TAP-1001-88"
                  className="w-full px-3 py-2 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 font-mono text-xs focus:ring-2 focus:ring-blue-500 outline-none"
                />
              </div>
            </div>

            <div className="flex justify-end space-x-2 pt-3">
              <button
                type="button"
                onClick={() => setIsModalOpen(false)}
                className="px-4 py-2 rounded-xl text-xs font-semibold text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-800"
              >
                Cancel
              </button>
              <button
                type="submit"
                className="px-4 py-2 rounded-xl text-xs font-semibold bg-blue-600 hover:bg-blue-700 text-white shadow-md"
              >
                Save Employee
              </button>
            </div>
          </form>
        </div>
      )}

      {/* Roster Table */}
      <div className="bg-white dark:bg-slate-900 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm overflow-hidden">
        <div className="overflow-x-auto">
          <table className="w-full text-left text-xs">
            <thead>
              <tr className="bg-slate-50 dark:bg-slate-800/70 border-b border-slate-200 dark:border-slate-800 text-slate-500 font-bold uppercase tracking-wider">
                <th className="p-4">Employee</th>
                <th className="p-4">Employee ID</th>
                <th className="p-4">Department</th>
                <th className="p-4">NFC UID</th>
                <th className="p-4">Barcode</th>
                <th className="p-4">Status</th>
                <th className="p-4 text-right">Actions</th>
              </tr>
            </thead>
            <tbody className="divide-y divide-slate-100 dark:divide-slate-800">
              {employees.map((emp) => (
                <tr key={emp.id} className="hover:bg-slate-50 dark:hover:bg-slate-800/50 transition">
                  <td className="p-4 font-semibold text-slate-800 dark:text-slate-200 flex items-center space-x-3">
                    <div className="w-8 h-8 rounded-full bg-slate-200 dark:bg-slate-700 text-slate-800 dark:text-slate-200 flex items-center justify-center font-bold text-xs">
                      {emp.fullName.substring(0, 2)}
                    </div>
                    <div>
                      <p className="font-bold">{emp.fullName}</p>
                      <p className="text-[10px] text-slate-400">{emp.phone}</p>
                    </div>
                  </td>
                  <td className="p-4 font-mono font-bold text-slate-900 dark:text-slate-100">{emp.id}</td>
                  <td className="p-4 text-slate-600 dark:text-slate-400">{emp.department}</td>
                  <td className="p-4 font-mono text-blue-600 dark:text-blue-400 font-semibold">{emp.nfcUid}</td>
                  <td className="p-4 font-mono text-purple-600 dark:text-purple-400 font-semibold">{emp.barcode}</td>
                  <td className="p-4">
                    <span className="px-2 py-0.5 bg-emerald-100 text-emerald-800 dark:bg-emerald-900/60 dark:text-emerald-300 rounded-full text-[10px] font-bold">
                      {emp.status}
                    </span>
                  </td>
                  <td className="p-4 text-right space-x-1">
                    <button
                      onClick={() => setActiveBarcodeEmp(emp)}
                      className="px-2 py-1 bg-purple-50 hover:bg-purple-100 dark:bg-purple-950/60 text-purple-700 dark:text-purple-300 rounded text-[10px] font-bold transition"
                      title="Generate Barcode Badge"
                    >
                      Badge
                    </button>
                    <button
                      onClick={() => openEditModal(emp)}
                      className="px-2 py-1 bg-slate-100 hover:bg-slate-200 dark:bg-slate-800 text-slate-700 dark:text-slate-300 rounded text-[10px] font-semibold transition"
                    >
                      Edit
                    </button>
                    <button
                      onClick={() => handleDelete(emp.id)}
                      className="px-2 py-1 bg-rose-50 hover:bg-rose-100 text-rose-600 rounded text-[10px] font-semibold transition"
                    >
                      Delete
                    </button>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </div>

    </div>
  );
}

function SettingsView({ settings, setSettings, showToast }) {
  const [form, setForm] = useState(settings);

  const handleSubmit = (e) => {
    e.preventDefault();
    setSettings(form);
    showToast('System configuration saved', 'success');
  };

  return (
    <div className="max-w-3xl space-y-6">
      <div className="bg-white dark:bg-slate-900 p-6 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-sm">
        <h3 className="font-bold text-slate-900 dark:text-white text-lg border-b pb-3 dark:border-slate-800">
          Organization & System Settings
        </h3>

        <form onSubmit={handleSubmit} className="mt-4 space-y-4">
          
          <div>
            <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Organization Name</label>
            <input
              type="text"
              value={form.orgName}
              onChange={(e) => setForm({ ...form, orgName: e.target.value })}
              className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-sm outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>

          <div className="grid grid-cols-2 gap-4">
            <div>
              <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">System Timezone</label>
              <input
                type="text"
                value={form.timezone}
                onChange={(e) => setForm({ ...form, timezone: e.target.value })}
                className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-sm outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>
            <div>
              <label className="block text-xs font-semibold text-slate-700 dark:text-slate-300 mb-1">Attendance Start Time (Late Threshold)</label>
              <input
                type="time"
                value={form.startTime}
                onChange={(e) => setForm({ ...form, startTime: e.target.value })}
                className="w-full px-3.5 py-2.5 rounded-xl bg-slate-50 dark:bg-slate-800 border border-slate-300 dark:border-slate-700 text-sm outline-none focus:ring-2 focus:ring-blue-500"
              />
            </div>
          </div>

          <div className="pt-3 border-t border-slate-100 dark:border-slate-800 space-y-3">
            <label className="flex items-center space-x-3 cursor-pointer">
              <input
                type="checkbox"
                checked={form.enableBarcodeBackup}
                onChange={(e) => setForm({ ...form, enableBarcodeBackup: e.target.checked })}
                className="w-4 h-4 rounded text-blue-600 focus:ring-blue-500"
              />
              <span className="text-xs font-semibold text-slate-700 dark:text-slate-300">
                Enable Barcode / QR Code Backup Terminal Input
              </span>
            </label>

            <label className="flex items-center space-x-3 cursor-pointer">
              <input
                type="checkbox"
                checked={form.playSoundFeedback}
                onChange={(e) => setForm({ ...form, playSoundFeedback: e.target.checked })}
                className="w-4 h-4 rounded text-blue-600 focus:ring-blue-500"
              />
              <span className="text-xs font-semibold text-slate-700 dark:text-slate-300">
                Play Audio Chime Feedback on NFC / Barcode Tap
              </span>
            </label>
          </div>

          <div className="pt-4 flex justify-end">
            <button
              type="submit"
              className="px-6 py-2.5 bg-blue-600 hover:bg-blue-700 text-white font-semibold text-xs rounded-xl shadow-md shadow-blue-500/20 transition"
            >
              Save System Settings
            </button>
          </div>

        </form>
      </div>
    </div>
  );
}
