import React, { useState, useEffect } from 'react';
import { Calendar, Users, Camera, Shield, BarChart3, Bell, User, Settings, CheckCircle, XCircle, AlertTriangle, Clock, MapPin, BookOpen } from 'lucide-react';
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, PieChart, Pie, Cell } from 'recharts';

const App = () => {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [currentSession, setCurrentSession] = useState(null);
  const [attendanceData, setAttendanceData] = useState([]);
  const [students, setStudents] = useState([
    { id: 1, name: 'Alice Johnson', rollNo: 'CS2023001', course: 'Computer Science', section: 'A', overallAttendance: 92, status: 'present' },
    { id: 2, name: 'Bob Smith', rollNo: 'CS2023002', course: 'Computer Science', section: 'A', overallAttendance: 78, status: 'absent' },
    { id: 3, name: 'Carol Davis', rollNo: 'CS2023003', course: 'Computer Science', section: 'A', overallAttendance: 85, status: 'present' },
    { id: 4, name: 'David Wilson', rollNo: 'CS2023004', course: 'Computer Science', section: 'A', overallAttendance: 65, status: 'absent' },
    { id: 5, name: 'Eva Brown', rollNo: 'CS2023005', course: 'Computer Science', section: 'A', overallAttendance: 95, status: 'present' },
  ]);

  const [sessions] = useState([
    { id: 1, course: 'CS-101', section: 'A', room: 'Lab-201', startTime: '09:00 AM', endTime: '10:30 AM', totalStudents: 45, present: 42, status: 'completed' },
    { id: 2, course: 'CS-201', section: 'B', room: 'Lab-202', startTime: '11:00 AM', endTime: '12:30 PM', totalStudents: 38, present: 35, status: 'completed' },
    { id: 3, course: 'CS-301', section: 'A', room: 'Lab-203', startTime: '02:00 PM', endTime: '03:30 PM', totalStudents: 41, present: 38, status: 'in-progress' },
  ]);

  const attendanceStats = [
    { name: 'Mon', attendance: 85 },
    { name: 'Tue', attendance: 92 },
    { name: 'Wed', attendance: 78 },
    { name: 'Thu', attendance: 88 },
    { name: 'Fri', attendance: 95 },
  ];

  const departmentStats = [
    { name: 'Computer Science', value: 88, color: '#3B82F6' },
    { name: 'Electronics', value: 82, color: '#10B981' },
    { name: 'Mechanical', value: 79, color: '#F59E0B' },
    { name: 'Civil', value: 85, color: '#EF4444' },
  ];

  const startSession = (session) => {
    setCurrentSession(session);
    setActiveTab('session');
  };

  const endSession = () => {
    setCurrentSession(null);
    setActiveTab('dashboard');
  };

  const Sidebar = () => (
    <div className="w-64 bg-gray-900 text-white min-h-screen p-6">
      <div className="mb-8">
        <h1 className="text-2xl font-bold text-blue-400">AttendAI</h1>
        <p className="text-gray-400 text-sm">Attendance Management System</p>
      </div>
      
      <nav className="space-y-2">
        {[
          { id: 'dashboard', label: 'Dashboard', icon: BarChart3 },
          { id: 'sessions', label: 'Sessions', icon: Calendar },
          { id: 'students', label: 'Students', icon: Users },
          { id: 'reports', label: 'Reports', icon: BarChart3 },
          { id: 'alerts', label: 'Alerts', icon: Bell },
          { id: 'settings', label: 'Settings', icon: Settings },
        ].map((item) => (
          <button
            key={item.id}
            onClick={() => setActiveTab(item.id)}
            className={`w-full flex items-center space-x-3 px-4 py-3 rounded-lg transition-colors ${
              activeTab === item.id 
                ? 'bg-blue-600 text-white' 
                : 'text-gray-300 hover:bg-gray-800'
            }`}
          >
            <item.icon size={20} />
            <span>{item.label}</span>
          </button>
        ))}
      </nav>
    </div>
  );

  const Dashboard = () => (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h2 className="text-3xl font-bold text-gray-900">Dashboard</h2>
        <div className="flex items-center space-x-4">
          <div className="flex items-center space-x-2 text-gray-600">
            <Clock size={16} />
            <span>{new Date().toLocaleDateString()}</span>
          </div>
        </div>
      </div>

      {/* Stats Cards */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <div className="bg-white rounded-xl shadow-lg p-6 border-l-4 border-blue-500">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 text-sm">Total Students</p>
              <p className="text-3xl font-bold text-gray-900">1,247</p>
            </div>
            <Users className="text-blue-500" size={32} />
          </div>
        </div>
        
        <div className="bg-white rounded-xl shadow-lg p-6 border-l-4 border-green-500">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 text-sm">Avg Attendance</p>
              <p className="text-3xl font-bold text-gray-900">87%</p>
            </div>
            <CheckCircle className="text-green-500" size={32} />
          </div>
        </div>
        
        <div className="bg-white rounded-xl shadow-lg p-6 border-l-4 border-yellow-500">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 text-sm">Active Sessions</p>
              <p className="text-3xl font-bold text-gray-900">3</p>
            </div>
            <Camera className="text-yellow-500" size={32} />
          </div>
        </div>
        
        <div className="bg-white rounded-xl shadow-lg p-6 border-l-4 border-red-500">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-gray-600 text-sm">Low Attendance</p>
              <p className="text-3xl font-bold text-gray-900">12</p>
            </div>
            <AlertTriangle className="text-red-500" size={32} />
          </div>
        </div>
      </div>

      {/* Charts */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">Weekly Attendance Trend</h3>
          <ResponsiveContainer width="100%" height={300}>
            <LineChart data={attendanceStats}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis />
              <Tooltip />
              <Line type="monotone" dataKey="attendance" stroke="#3B82F6" strokeWidth={3} />
            </LineChart>
          </ResponsiveContainer>
        </div>

        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">Department-wise Attendance</h3>
          <ResponsiveContainer width="100%" height={300}>
            <PieChart>
              <Pie
                data={departmentStats}
                cx="50%"
                cy="50%"
                outerRadius={100}
                dataKey="value"
                label={({name, percent}) => `${name}: ${(percent * 100).toFixed(0)}%`}
              >
                {departmentStats.map((entry, index) => (
                  <Cell key={`cell-${index}`} fill={entry.color} />
                ))}
              </Pie>
              <Tooltip />
            </PieChart>
          </ResponsiveContainer>
        </div>
      </div>

      {/* Today's Sessions */}
      <div className="bg-white rounded-xl shadow-lg p-6">
        <h3 className="text-xl font-semibold mb-4">Today's Sessions</h3>
        <div className="space-y-4">
          {sessions.map((session) => (
            <div key={session.id} className="flex items-center justify-between p-4 border rounded-lg hover:bg-gray-50">
              <div className="flex items-center space-x-4">
                <div className={`p-3 rounded-lg ${
                  session.status === 'in-progress' ? 'bg-yellow-100' : 'bg-green-100'
                }`}>
                  <BookOpen className={
                    session.status === 'in-progress' ? 'text-yellow-600' : 'text-green-600'
                  } size={24} />
                </div>
                <div>
                  <h4 className="font-semibold">{session.course} - Section {session.section}</h4>
                  <div className="flex items-center space-x-4 text-sm text-gray-600">
                    <span className="flex items-center space-x-1">
                      <MapPin size={14} />
                      <span>{session.room}</span>
                    </span>
                    <span className="flex items-center space-x-1">
                      <Clock size={14} />
                      <span>{session.startTime} - {session.endTime}</span>
                    </span>
                  </div>
                </div>
              </div>
              <div className="text-right">
                <div className="text-lg font-semibold">
                  {session.present}/{session.totalStudents}
                </div>
                <div className="text-sm text-gray-600">Present</div>
                {session.status === 'in-progress' && (
                  <button
                    onClick={() => startSession(session)}
                    className="mt-2 px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors"
                  >
                    Join Session
                  </button>
                )}
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );

  const SessionView = () => (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <div>
          <h2 className="text-3xl font-bold text-gray-900">Session in Progress</h2>
          <p className="text-gray-600">
            {currentSession?.course} - Section {currentSession?.section} | Room {currentSession?.room}
          </p>
        </div>
        <button
          onClick={endSession}
          className="px-6 py-3 bg-red-600 text-white rounded-lg hover:bg-red-700 transition-colors flex items-center space-x-2"
        >
          <XCircle size={20} />
          <span>End Session</span>
        </button>
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        {/* Camera Feed */}
        <div className="lg:col-span-2 bg-gray-900 rounded-xl p-4">
          <div className="aspect-video bg-gray-800 rounded-lg flex items-center justify-center relative">
            <Camera size={64} className="text-gray-600" />
            <div className="absolute top-4 left-4 bg-red-600 text-white px-3 py-1 rounded-full text-sm font-semibold">
              LIVE
            </div>
            <div className="absolute bottom-4 left-4 text-white">
              <div className="flex items-center space-x-2">
                <div className="w-3 h-3 bg-red-500 rounded-full animate-pulse"></div>
                <span>Recording</span>
              </div>
            </div>
          </div>
          
          {/* Detected Students */}
          <div className="mt-4 grid grid-cols-2 md:grid-cols-3 gap-3">
            {students.slice(0, 6).map((student) => (
              <div key={student.id} className="bg-gray-800 rounded-lg p-3 text-white">
                <div className="flex items-center space-x-2">
                  <div className="w-8 h-8 bg-green-500 rounded-full flex items-center justify-center">
                    <CheckCircle size={16} />
                  </div>
                  <div>
                    <div className="font-semibold text-sm">{student.name.split(' ')[0]}</div>
                    <div className="text-xs text-gray-400">{student.rollNo}</div>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* Attendance List */}
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">Attendance Status</h3>
          <div className="space-y-3 max-h-96 overflow-y-auto">
            {students.map((student) => (
              <div key={student.id} className="flex items-center justify-between p-3 border rounded-lg">
                <div>
                  <div className="font-semibold">{student.name}</div>
                  <div className="text-sm text-gray-600">{student.rollNo}</div>
                </div>
                <div className={`px-3 py-1 rounded-full text-sm font-semibold ${
                  student.status === 'present' 
                    ? 'bg-green-100 text-green-800' 
                    : 'bg-red-100 text-red-800'
                }`}>
                  {student.status === 'present' ? 'Present' : 'Absent'}
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );

  const StudentsView = () => (
    <div className="space-y-6">
      <div className="flex justify-between items-center">
        <h2 className="text-3xl font-bold text-gray-900">Student Management</h2>
        <button className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors">
          Add Student
        </button>
      </div>

      <div className="bg-white rounded-xl shadow-lg overflow-hidden">
        <div className="p-6 border-b">
          <div className="flex items-center space-x-4">
            <input
              type="text"
              placeholder="Search students..."
              className="flex-1 px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
            <select className="px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
              <option>All Courses</option>
              <option>Computer Science</option>
              <option>Electronics</option>
              <option>Mechanical</option>
            </select>
          </div>
        </div>
        
        <div className="overflow-x-auto">
          <table className="w-full">
            <thead className="bg-gray-50">
              <tr>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Student</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Course</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Section</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Roll No</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Attendance %</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
              </tr>
            </thead>
            <tbody className="bg-white divide-y divide-gray-200">
              {students.map((student) => (
                <tr key={student.id} className="hover:bg-gray-50">
                  <td className="px-6 py-4 whitespace-nowrap">
                    <div className="flex items-center">
                      <div className="w-10 h-10 bg-gray-300 rounded-full flex items-center justify-center">
                        <User size={20} className="text-gray-600" />
                      </div>
                      <div className="ml-4">
                        <div className="text-sm font-medium text-gray-900">{student.name}</div>
                      </div>
                    </div>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{student.course}</td>
                  <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{student.section}</td>
                  <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-900">{student.rollNo}</td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <div className="flex items-center">
                      <div className="text-sm font-medium text-gray-900">{student.overallAttendance}%</div>
                      <div className={`ml-2 w-16 h-2 rounded-full ${
                        student.overallAttendance >= 85 ? 'bg-green-200' : 
                        student.overallAttendance >= 75 ? 'bg-yellow-200' : 'bg-red-200'
                      }`}>
                        <div 
                          className={`h-2 rounded-full ${
                            student.overallAttendance >= 85 ? 'bg-green-500' : 
                            student.overallAttendance >= 75 ? 'bg-yellow-500' : 'bg-red-500'
                          }`}
                          style={{ width: `${student.overallAttendance}%` }}
                        ></div>
                      </div>
                    </div>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <span className={`px-2 inline-flex text-xs leading-5 font-semibold rounded-full ${
                      student.overallAttendance >= 75 
                        ? 'bg-green-100 text-green-800' 
                        : 'bg-red-100 text-red-800'
                    }`}>
                      {student.overallAttendance >= 75 ? 'Good' : 'At Risk'}
                    </span>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  );

  const ReportsView = () => (
    <div className="space-y-6">
      <h2 className="text-3xl font-bold text-gray-900">Reports & Analytics</h2>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">Attendance Overview</h3>
          <div className="space-y-4">
            <div className="flex justify-between items-center p-4 bg-blue-50 rounded-lg">
              <span className="font-medium">Overall Attendance Rate</span>
              <span className="text-2xl font-bold text-blue-600">87%</span>
            </div>
            <div className="flex justify-between items-center p-4 bg-green-50 rounded-lg">
              <span className="font-medium">Students Meeting Requirements</span>
              <span className="text-2xl font-bold text-green-600">89%</span>
            </div>
            <div className="flex justify-between items-center p-4 bg-yellow-50 rounded-lg">
              <span className="font-medium">At-Risk Students</span>
              <span className="text-2xl font-bold text-yellow-600">11%</span>
            </div>
          </div>
        </div>

        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">Department Performance</h3>
          <div className="space-y-3">
            {departmentStats.map((dept, index) => (
              <div key={index} className="flex items-center justify-between">
                <span className="font-medium">{dept.name}</span>
                <div className="flex items-center space-x-3">
                  <div className="w-32 h-3 bg-gray-200 rounded-full">
                    <div 
                      className="h-3 rounded-full"
                      style={{ width: `${dept.value}%`, backgroundColor: dept.color }}
                    ></div>
                  </div>
                  <span className="font-semibold">{dept.value}%</span>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>

      <div className="bg-white rounded-xl shadow-lg p-6">
        <h3 className="text-xl font-semibold mb-4">Detailed Analytics</h3>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div className="text-center p-6 border rounded-lg">
            <div className="text-3xl font-bold text-blue-600 mb-2">92%</div>
            <div className="text-gray-600">Monday Attendance</div>
          </div>
          <div className="text-center p-6 border rounded-lg">
            <div className="text-3xl font-bold text-green-600 mb-2">88%</div>
            <div className="text-gray-600">Tuesday Attendance</div>
          </div>
          <div className="text-center p-6 border rounded-lg">
            <div className="text-3xl font-bold text-yellow-600 mb-2">78%</div>
            <div className="text-gray-600">Wednesday Attendance</div>
          </div>
        </div>
      </div>
    </div>
  );

  const AlertsView = () => (
    <div className="space-y-6">
      <h2 className="text-3xl font-bold text-gray-900">Alerts & Notifications</h2>
      
      <div className="space-y-4">
        <div className="bg-yellow-50 border-l-4 border-yellow-400 p-4 rounded-r-lg">
          <div className="flex items-start space-x-3">
            <AlertTriangle className="text-yellow-600 mt-1" size={20} />
            <div>
              <h3 className="font-semibold text-yellow-800">Low Attendance Alert</h3>
              <p className="text-yellow-700">David Wilson (CS2023004) attendance in CS-101 has dropped to 65%</p>
              <p className="text-sm text-yellow-600 mt-1">2 hours ago</p>
            </div>
          </div>
        </div>

        <div className="bg-red-50 border-l-4 border-red-400 p-4 rounded-r-lg">
          <div className="flex items-start space-x-3">
            <AlertTriangle className="text-red-600 mt-1" size={20} />
            <div>
              <h3 className="font-semibold text-red-800">Critical Alert</h3>
              <p className="text-red-700">3 students in CS-201 Section B are below minimum attendance requirement</p>
              <p className="text-sm text-red-600 mt-1">1 day ago</p>
            </div>
          </div>
        </div>

        <div className="bg-blue-50 border-l-4 border-blue-400 p-4 rounded-r-lg">
          <div className="flex items-start space-x-3">
            <Bell className="text-blue-600 mt-1" size={20} />
            <div>
              <h3 className="font-semibold text-blue-800">System Notification</h3>
              <p className="text-blue-700">New attendance session started for CS-301 Section A</p>
              <p className="text-sm text-blue-600 mt-1">2 days ago</p>
            </div>
          </div>
        </div>

        <div className="bg-green-50 border-l-4 border-green-400 p-4 rounded-r-lg">
          <div className="flex items-start space-x-3">
            <CheckCircle className="text-green-600 mt-1" size={20} />
            <div>
              <h3 className="font-semibold text-green-800">Attendance Updated</h3>
              <p className="text-green-700">Alice Johnson marked present for CS-101 session</p>
              <p className="text-sm text-green-600 mt-1">3 days ago</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  );

  const SettingsView = () => (
    <div className="space-y-6">
      <h2 className="text-3xl font-bold text-gray-900">System Settings</h2>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">General Settings</h3>
          <div className="space-y-4">
            <div className="flex items-center justify-between">
              <span className="font-medium">Minimum Attendance Requirement</span>
              <input
                type="number"
                defaultValue="75"
                className="w-20 px-3 py-2 border rounded-lg text-center"
              />
            </div>
            <div className="flex items-center justify-between">
              <span className="font-medium">Enable Facial Recognition</span>
              <div className="relative inline-block w-12 h-6">
                <input type="checkbox" className="sr-only" defaultChecked />
                <div className="block w-12 h-6 rounded-full bg-blue-600"></div>
                <div className="absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-transform transform translate-x-6"></div>
              </div>
            </div>
            <div className="flex items-center justify-between">
              <span className="font-medium">Enable Parent Notifications</span>
              <div className="relative inline-block w-12 h-6">
                <input type="checkbox" className="sr-only" />
                <div className="block w-12 h-6 rounded-full bg-gray-300"></div>
                <div className="absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-transform"></div>
              </div>
            </div>
          </div>
        </div>

        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-semibold mb-4">Security Settings</h3>
          <div className="space-y-4">
            <div className="flex items-center justify-between">
              <span className="font-medium">Biometric Data Encryption</span>
              <Shield className="text-green-600" size={24} />
            </div>
            <div className="flex items-center justify-between">
              <span className="font-medium">Two-Factor Authentication</span>
              <div className="relative inline-block w-12 h-6">
                <input type="checkbox" className="sr-only" defaultChecked />
                <div className="block w-12 h-6 rounded-full bg-blue-600"></div>
                <div className="absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-transform transform translate-x-6"></div>
              </div>
            </div>
            <div className="flex items-center justify-between">
              <span className="font-medium">Audit Logging</span>
              <div className="relative inline-block w-12 h-6">
                <input type="checkbox" className="sr-only" defaultChecked />
                <div className="block w-12 h-6 rounded-full bg-blue-600"></div>
                <div className="absolute left-1 top-1 bg-white w-4 h-4 rounded-full transition-transform transform translate-x-6"></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div className="bg-white rounded-xl shadow-lg p-6">
        <h3 className="text-xl font-semibold mb-4">System Information</h3>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div className="text-center p-4 border rounded-lg">
            <div className="text-2xl font-bold text-gray-900">1,247</div>
            <div className="text-gray-600">Total Students</div>
          </div>
          <div className="text-center p-4 border rounded-lg">
            <div className="text-2xl font-bold text-gray-900">48</div>
            <div className="text-gray-600">Active Courses</div>
          </div>
          <div className="text-center p-4 border rounded-lg">
            <div className="text-2xl font-bold text-gray-900">156</div>
            <div className="text-gray-600">Active Sessions</div>
          </div>
        </div>
      </div>
    </div>
  );

  const renderContent = () => {
    switch (activeTab) {
      case 'dashboard':
        return <Dashboard />;
      case 'session':
        return <SessionView />;
      case 'students':
        return <StudentsView />;
      case 'reports':
        return <ReportsView />;
      case 'alerts':
        return <AlertsView />;
      case 'settings':
        return <SettingsView />;
      default:
        return <Dashboard />;
    }
  };

  return (
    <div className="flex min-h-screen bg-gray-100">
      <Sidebar />
      <main className="flex-1 p-8 overflow-auto">
        {renderContent()}
      </main>
    </div>
  );
};

export default App;