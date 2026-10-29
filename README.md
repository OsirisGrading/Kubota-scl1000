# Kubota-scl1000
Kubota rental agreement


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kubota SCL1000 Rental Portal & Agreement</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        kubota: '#F36C21',
                        kubotadark: '#D4530C',
                        charcoal: '#222222',
                        darkgray: '#333333'
                    }
                }
            }
        }
    </script>
    <style>
        .calendar-day.disabled {
            background-color: #Fee2e2;
            color: #ef4444;
            cursor: not-allowed;
            text-decoration: line-through;
        }
        .calendar-day.selected {
            background-color: #F36C21;
            color: white;
            font-weight: bold;
        }
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background: white; color: black; }
        }
    </style>
</head>
<body class="bg-slate-100 font-sans text-gray-800 min-h-screen">

    <!-- Header / Branding -->
    <header class="bg-charcoal text-white border-b-4 border-kubota shadow-md sticky top-0 z-50">
        <div class="max-w-4xl mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-kubota text-white p-2 rounded-lg font-black text-xl tracking-wider">KUBOTA</div>
                <div>
                    <h1 class="font-bold text-lg leading-tight">SCL1000 Equipment Rental</h1>
                    <p class="text-xs text-gray-400">Official Portal & Digital E-Sign Contract</p>
                </div>
            </div>
            <a href="tel:" class="no-print bg-darkgray hover:bg-gray-700 text-xs px-3 py-2 rounded text-gray-200 flex items-center gap-2 border border-gray-600">
                <i class="fa-solid fa-phone text-kubota"></i> Contact Owner
            </a>
        </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 py-6">

        <!-- Machine Card Highlight -->
        <div class="bg-white rounded-xl shadow-sm border border-gray-200 overflow-hidden mb-6 flex flex-col md:flex-row">
            <div class="bg-charcoal text-white p-6 md:w-1/3 flex flex-col justify-between relative overflow-hidden">
                <div class="z-10">
                    <span class="bg-kubota text-xs font-bold px-2.5 py-1 rounded-full uppercase tracking-wider">Stand-On CTL</span>
                    <h2 class="text-2xl font-black mt-2">Kubota SCL1000</h2>
                    <p class="text-xs text-gray-400 mt-1">Compact Track Loader</p>
                </div>
                <div class="my-4 z-10 space-y-2 text-xs">
                    <div class="flex justify-between border-b border-gray-700 pb-1">
                        <span class="text-gray-400">Rated Operating Cap:</span>
                        <span class="font-bold text-white">1,000 lbs</span>
                    </div>
                    <div class="flex justify-between border-b border-gray-700 pb-1">
                        <span class="text-gray-400">Operating Weight:</span>
                        <span class="font-bold text-white">3,035 lbs</span>
                    </div>
                    <div class="flex justify-between border-b border-gray-700 pb-1">
                        <span class="text-gray-400">Engine Fuel:</span>
                        <span class="font-bold text-kubota">Ultra-Low Sulfur Diesel Only</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-gray-400">DEF Fluid Required?</span>
                        <span class="font-bold text-green-400">NO DEF REQUIRED</span>
                    </div>
                </div>
                <div class="text-[10px] text-gray-500 z-10">Keyless Start Password Provided at Handover</div>
            </div>

            <!-- Booking Calendar / Availability Container -->
            <div class="p-6 md:w-2/3 flex flex-col justify-between">
                <div>
                    <div class="flex justify-between items-center mb-3">
                        <h3 class="font-bold text-gray-900 flex items-center gap-2">
                            <i class="fa-solid fa-calendar-days text-kubota"></i> Real-Time Availability Calendar
                        </h3>
                        <span id="firebaseStatus" class="text-[11px] bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-medium">Live Sync Active</span>
                    </div>
                    
                    <!-- Month Header -->
                    <div class="flex justify-between items-center bg-gray-50 p-2 rounded-lg border border-gray-200 mb-2 text-sm font-bold">
                        <button id="prevMonth" class="px-2 py-1 text-gray-600 hover:text-black">&lt;</button>
                        <span id="calendarMonthYear">Current Month</span>
                        <button id="nextMonth" class="px-2 py-1 text-gray-600 hover:text-black">&gt;</button>
                    </div>

                    <!-- Calendar Grid -->
                    <div class="grid grid-cols-7 gap-1 text-center text-xs font-semibold text-gray-500 mb-1">
                        <div>Su</div><div>Mo</div><div>Tu</div><div>We</div><div>Th</div><div>Fr</div><div>Sa</div>
                    </div>
                    <div id="calendarDays" class="grid grid-cols-7 gap-1 text-center text-xs">
                        <!-- Rendered via Javascript -->
                    </div>

                    <div class="flex items-center justify-end gap-4 mt-3 text-[11px] text-gray-500">
                        <div class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-white border border-gray-300 inline-block rounded-sm"></span> Available</div>
                        <div class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-red-200 border border-red-400 inline-block rounded-sm"></span> Booked / Rented</div>
                        <div class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-kubota inline-block rounded-sm">
