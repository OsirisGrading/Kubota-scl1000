<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Osiris Grading & Rental | Kubota SCL1000 Portal</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        kubota: '#F36C21',
                        kubotadark: '#D4530C',
                        darkbg: '#121417',
                        cardbg: '#1E2124',
                        cardborder: '#2A2E33',
                        lighttext: '#E2E8F0',
                        mutedtext: '#94A3B8'
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #121417;
            background-image: radial-gradient(rgba(243, 108, 33, 0.05) 1px, transparent 0);
            background-size: 24px 24px;
        }
        .calendar-day.disabled {
            background-color: #2D1A1A;
            color: #EF4444;
            cursor: not-allowed;
            text-decoration: line-through;
            border-color: #450A0A;
        }
        .calendar-day.selected-endpoint {
            background-color: #F36C21 !important;
            color: #FFFFFF !important;
            font-weight: bold;
            border-color: #F36C21 !important;
        }
        .calendar-day.in-range {
            background-color: rgba(243, 108, 33, 0.25) !important;
            color: #FFEDD5 !important;
            border-color: rgba(243, 108, 33, 0.4) !important;
        }
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background: white !important; color: black !important; }
            .dark-card { background: white !important; border: 1px solid #ccc !important; color: black !important; }
        }
    </style>
</head>
<body class="bg-darkbg font-sans text-lighttext min-h-screen">

    <!-- Header / Branding -->
    <header class="bg-cardbg/90 backdrop-blur-md text-white border-b-2 border-kubota shadow-xl sticky top-0 z-50">
        <div class="max-w-4xl mx-auto px-4 py-3 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-kubota text-white px-3 py-1 rounded font-black text-xl tracking-wider shadow-md">OGR</div>
                <div>
                    <h1 class="font-bold text-base leading-tight text-white uppercase tracking-wide">Osiris Grading & Rental</h1>
                    <p class="text-[11px] text-mutedtext">Georgia Equipment Rental & Services</p>
                </div>
            </div>
            <a href="tel:4049510578" class="no-print bg-darkbg hover:bg-cardborder text-xs px-3 py-2 rounded-lg text-gray-200 flex items-center gap-2 border border-cardborder transition">
                <i class="fa-solid fa-phone text-kubota"></i> <span class="hidden sm:inline">404-951-0578</span>
            </a>
        </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 py-6">

        <!-- Georgia Rental Rates Header -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-3 mb-6 no-print">
            <div class="bg-cardbg border border-cardborder rounded-xl p-3 flex items-center gap-3">
                <div class="w-10 h-10 rounded-lg bg-kubota/10 border border-kubota/30 flex items-center justify-center text-kubota font-bold text-lg">24h</div>
                <div>
                    <div class="text-[10px] uppercase font-bold text-mutedtext">Daily Rate (Machine Only)</div>
                    <div class="text-xl font-black text-white">$250.00 <span class="text-xs font-normal text-mutedtext">/ day</span></div>
                    <div class="text-[10px] text-kubota">Return 24h later or by 8 AM</div>
                </div>
            </div>
            <div class="bg-cardbg border border-cardborder rounded-xl p-3 flex items-center gap-3">
                <div class="w-10 h-10 rounded-lg bg-kubota/10 border border-kubota/30 flex items-center justify-center text-kubota font-bold text-lg">👷</div>
                <div>
                    <div class="text-[10px] uppercase font-bold text-mutedtext">Machine + Operator</div>
                    <div class="text-xl font-black text-white">$700.00 <span class="text-xs font-normal text-mutedtext">/ day</span></div>
                    <div class="text-[10px] text-green-400">Includes Certified Operator</div>
                </div>
            </div>
            <div class="bg-cardbg border border-cardborder rounded-xl p-3 flex items-center gap-3">
                <div class="w-10 h-10 rounded-lg bg-kubota/10 border border-kubota/30 flex items-center justify-center text-kubota font-bold text-lg">7d</div>
                <div>
                    <div class="text-[10px] uppercase font-bold text-mutedtext">Weekly Rate (Machine)</div>
                    <div class="text-xl font-black text-white">$875.00 <span class="text-xs font-normal text-mutedtext">/ week</span></div>
                    <div class="text-[10px] text-green-400">Monthly: $2,500.00</div>
                </div>
            </div>
        </div>

        <!-- Flexible Return Policy Banner -->
        <div class="bg-kubota/10 border border-kubota/40 rounded-xl p-3 mb-6 flex items-center gap-3 text-xs text-orange-200 no-print">
            <i class="fa-solid fa-clock text-kubota text-xl"></i>
            <div>
                <strong class="font-bold text-white">Flexible Return Schedule:</strong> Equipment rented for 24 hours can be returned either exactly 24 hours after pickup or early the following morning by 8:00 AM at no additional charge.
            </div>
        </div>

        <!-- Machine Highlights & Availability Calendar -->
        <div class="bg-cardbg rounded-2xl shadow-2xl border border-cardborder overflow-hidden mb-6 flex flex-col md:flex-row">
            <div class="bg-cardborder/40 p-6 md:w-1/3 flex flex-col justify-between border-b md:border-b-0 md:border-r border-cardborder relative">
                <div>
                    <span class="bg-kubota text-white text-[10px] font-black px-2.5 py-1 rounded-full uppercase tracking-wider">Stand-On CTL</span>
                    <h2 class="text-2xl font-black text-white mt-2">Kubota SCL1000</h2>
                    <p class="text-xs text-mutedtext mt-0.5">Osiris Rental Fleet</p>
                </div>
                <div class="my-4 space-y-2 text-xs">
                    <div class="flex justify-between border-b border-cardborder/60 pb-1.5">
                        <span class="text-mutedtext">Rated Operating Cap:</span>
                        <span class="font-bold text-white">1,000 lbs</span>
                    </div>
                    <div class="flex justify-between border-b border-cardborder/60 pb-1.5">
                        <span class="text-mutedtext">Operating Weight:</span>
                        <span class="font-bold text-white">3,035 lbs</span>
                    </div>
                    <div class="flex justify-between border-b border-cardborder/60 pb-1.5">
                        <span class="text-mutedtext">Fuel Requirement:</span>
                        <span class="font-bold text-kubota">Ultra-Low Sulfur Diesel</span>
                    </div>
                    <div class="flex justify-between border-b border-cardborder/60 pb-1.5">
                        <span class="text-mutedtext">DEF Required?</span>
                        <span class="font-bold text-green-400">NO DEF REQUIRED</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="text-mutedtext">Included Attachment:</span>
                        <span class="font-bold text-white">Tooth Bucket</span>
                    </div>
                </div>
                <div class="text-[10px] text-mutedtext bg-darkbg p-2 rounded-lg border border-cardborder">
                    <i class="fa-solid fa-key text-kubota mr-1"></i> Keyless passcode provided at pickup
                </div>
            </div>

            <!-- Booking Calendar Grid -->
            <div class="p-6 md:w-2/3 flex flex-col justify-between bg-cardbg">
                <div>
                    <div class="flex justify-between items-center mb-3">
                        <h3 class="font-bold text-white text-sm flex items-center gap-2">
                            <i class="fa-solid fa-calendar-days text-kubota"></i> Real-Time Availability Calendar
                        </h3>
                        <span id="firebaseStatus" class="text-[10px] bg-green-950 text-green-400 border border-green-800 px-2 py-0.5 rounded-full font-medium">Live Sync Active</span>
                    </div>
                    
                    <!-- Month Header -->
                    <div class="flex justify-between items-center bg-darkbg p-2 rounded-xl border border-cardborder mb-2 text-xs font-bold text-white">
                        <button id="prevMonth" type="button" class="px-3 py-1 hover:bg-cardborder rounded text-mutedtext hover:text-white transition">&lt;</button>
                        <span id="calendarMonthYear">Current Month</span>
                        <button id="nextMonth" type="button" class="px-3 py-1 hover:bg-cardborder rounded text-mutedtext hover:text-white transition">&gt;</button>
                    </div>

                    <!-- Calendar Grid -->
                    <div class="grid grid-cols-7 gap-1 text-center text-[10px] font-bold uppercase tracking-wider text-mutedtext mb-1">
                        <div>Su</div><div>Mo</div><div>Tu</div><div>We</div><div>Th</div><div>Fr</div><div>Sa</div>
                    </div>
                    <div id="calendarDays" class="grid grid-cols-7 gap-1 text-center text-xs">
                        <!-- Rendered via Javascript -->
                    </div>

                    <div class="flex items-center justify-end gap-3 mt-3 text-[10px] text-mutedtext">
                        <div class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-darkbg border border-cardborder inline-block rounded"></span> Available</div>
                        <div class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-red-950 border border-red-800 inline-block rounded"></span> Booked</div>
                        <div class="flex items-center gap-1"><span class="w-2.5 h-2.5 bg-kubota inline-block rounded"></span> Selected Range</div>
                    </div>
                </div>
            </div>
        </div>

        <!-- RENTAL FORM -->
        <form id="rentalForm" class="space-y-6">

            <!-- 1. Renter Details -->
            <div class="bg-cardbg p-6 rounded-2xl border border-cardborder shadow-xl">
                <h3 class="text-sm font-bold text-white mb-4 pb-2 border-b border-cardborder flex items-center gap-2">
                    <span class="bg-kubota text-white w-5 h-5 rounded-full inline-flex items-center justify-center text-xs font-black">1</span> 
                    Renter / Lessee Details
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Full Legal Name *</label>
                        <input type="text" id="renterName" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition" placeholder="First and Last Name">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Phone Number *</label>
                        <input type="tel" id="renterPhone" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition" placeholder="(555) 000-0000">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Driver's License # & State *</label>
                        <input type="text" id="renterDL" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition" placeholder="e.g. DL12345678 - GA">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Billing / Home Address *</label>
                        <input type="text" id="renterAddress" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition" placeholder="Street, City, State, Zip">
                    </div>
                </div>
            </div>

            <!-- 2. Dates & Machine Setup -->
            <div class="bg-cardbg p-6 rounded-2xl border border-cardborder shadow-xl">
                <h3 class="text-sm font-bold text-white mb-4 pb-2 border-b border-cardborder flex items-center gap-2">
                    <span class="bg-kubota text-white w-5 h-5 rounded-full inline-flex items-center justify-center text-xs font-black">2</span> 
                    Rental Terms & Operator Options
                </h3>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Start Date *</label>
                        <input type="date" id="startDate" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Return Date *</label>
                        <input type="date" id="endDate" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-mutedtext mb-1">Starting Engine Hours *</label>
                        <input type="number" step="0.1" id="startHours" required class="w-full p-2.5 text-sm bg-darkbg border border-cardborder rounded-xl text-white focus:border-kubota outline-none transition" placeholder="From SCL1000 Screen">
                    </div>
                </div>

                <!-- Operator Request Checkbox -->
                <div class="p-4 bg-darkbg border border-cardborder rounded-xl mb-4">
                    <label class="flex items-center gap-3 cursor-pointer">
                        <input type="checkbox" id="includeOperator" class="w-5 h-5 accent-kubota rounded cursor-pointer">
                        <div>
                            <span class="font-bold text-white text-sm">Request Certified Operator (+$450.00/day)</span>
                            <p class="text-xs text-mutedtext">Includes machine + professional operator for <strong>$700.00 / day total</strong>.</p>
                        </div>
                    </label>
                </div>

                <div class="pt-4 border-t border-cardborder">
                    <label class="block text-xs font-bold text-mutedtext mb-2">Included Machine Attachment:</label>
                    <div class="p-3 bg-darkbg border border-cardborder rounded-xl flex items-center gap-3">
                        <i class="fa-solid fa-square-check text-kubota text-lg"></i>
                        <div>
                            <span class="font-bold text-white text-xs">Standard Tooth Bucket</span>
                            <p class="text-[11px] text-mutedtext">Pre-installed for excavation, digging, and tough soil transport.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 3. Pre-Rental Safety Verification -->
            <div class="bg-cardbg p-6 rounded-2xl border border-cardborder shadow-xl">
                <h3 class="text-sm font-bold text-white mb-4 pb-2 border-b border-cardborder flex items-center gap-2">
                    <span class="bg-kubota text-white w-5 h-5 rounded-full inline-flex items-center justify-center text-xs font-black">3</span> 
                    Pre-Rental Inspection & Fluid Acknowledgment
                </h3>
                <div class="space-y-3 text-xs">
                    <label class="flex items-start gap-3 p-3 bg-amber-950/20 border border-amber-800/40 rounded-xl cursor-pointer">
                        <input type="checkbox" required class="mt-0.5 accent-kubota">
                        <div>
                            <span class="font-bold text-amber-300">Ultra-Low Sulfur Diesel Only (NO DEF REQUIRED)</span>
                            <p class="text-amber-200/70">I acknowledge this Kubota SCL1000 runs strictly on clean Ultra-Low Sulfur Diesel. Putting gasoline, contaminated fuel, or DEF into the fuel tank will cause engine destruction, and I will be 100% financially liable for all repair and flush costs.</p>
                        </div>
                    </label>

                    <label class="flex items-start gap-3 p-3 bg-darkbg border border-cardborder rounded-xl cursor-pointer">
                        <input type="checkbox" required class="mt-0.5 accent-kubota">
                        <div>
                            <span class="font-bold text-white">Fuel Level Return Obligation</span>
                            <p class="text-mutedtext">The machine is provided with a FULL tank (7.75 Gallons). I agree to return it fully topped off with Diesel. If returned with less fuel, I authorize a refueling fee of $10.00 per missing gallon.</p>
                        </div>
                    </label>

                    <label class="flex items-start gap-3 p-3 bg-darkbg border border-cardborder rounded-xl cursor-pointer">
                        <input type="checkbox" required class="mt-0.5 accent-kubota">
                        <div>
                            <span class="font-bold text-white">Rubber Tracks & Undercarriage Condition</span>
                            <p class="text-mutedtext">I have inspected the rubber tracks and confirm there are no existing tears or thrown tracks. I accept full liability for track damage caused by sharp debris, rebar, or misuse during my rental period.</p>
                        </div>
                    </label>

                    <label class="flex items-start gap-3 p-3 bg-darkbg border border-cardborder rounded-xl cursor-pointer">
                        <input type="checkbox" required class="mt-0.5 accent-kubota">
                        <div>
                            <span class="font-bold text-white">Cleaning Fee Policy</span>
                            <p class="text-mutedtext">Equipment must be returned reasonably clean. A minimum $150 cleaning fee will apply if heavy detailing or washing is required upon return.</p>
                        </div>
                    </label>
                </div>
            </div>

            <!-- 4. Terms & Conditions -->
            <div class="bg-cardbg p-6 rounded-2xl border border-cardborder shadow-xl">
                <h3 class="text-sm font-bold text-white mb-2 flex items-center gap-2">
                    <span class="bg-kubota text-white w-5 h-5 rounded-full inline-flex items-center justify-center text-xs font-black">4</span> 
                    Osiris Grading & Rental Terms & Agreement
                </h3>
                <p class="text-xs text-mutedtext mb-3">Please scroll and review the agreement terms before signing.</p>
                
                <div class="h-44 overflow-y-scroll p-4 bg-darkbg border border-cardborder rounded-xl text-xs leading-relaxed text-mutedtext space-y-3">
                    <p class="font-bold uppercase text-white border-b border-cardborder pb-1">OSIRIS GRADING & RENTAL - KUBOTA SCL1000 AGREEMENT</p>
                    <p><strong>1. FINANCIAL RESPONSIBILITY:</strong> Renter assumes 100% financial liability for all damage, loss, theft, vandalism, fire, or breakdown resulting from improper operation or abuse during the rental period.</p>
                    <p><strong>2. OPERATOR QUALIFICATIONS:</strong> Only trained, competent adult operators (18+) named in this contract are authorized to operate the equipment. Subleasing is strictly prohibited. Maximum Rated Operating Capacity is 1,000 lbs.</p>
                    <p><strong>3. HOURLY ALLOWANCE & OVERTIME:</strong> Daily rental includes up to 8 engine operating hours per 24-hour cycle. Any excess hours recorded on the digital hour meter upon return will be charged at $25.00 per excess hour.</p>
                    <p><strong>4. RETURN TIMING:</strong> Renter agrees to return the equipment within 24 hours of pickup or by 8:00 AM the following morning unless an extension is granted and authorized in writing by Osiris Grading & Rental.</p>
                    <p><strong>5. INDEMNIFICATION:</strong> Renter releases Osiris Grading & Rental from all liability regarding personal injury, property damage, downtime, or job delays arising from equipment transport or operation.</p>
                </div>
            </div>

            <!-- 5. Financial Calculation Breakdown -->
            <div class="bg-cardbg p-6 rounded-2xl border border-cardborder shadow-xl">
                <h3 class="text-sm font-bold text-white mb-3 pb-2 border-b border-cardborder flex items-center justify-between">
                    <span class="flex items-center gap-2">
                        <span class="bg-kubota text-white w-5 h-5 rounded-full inline-flex items-center justify-center text-xs font-black">5</span> 
                        Financial Summary & Final Price
                    </span>
                    <span class="text-xs text-kubota font-normal">Georgia Tax Included</span>
                </h3>

                <div class="space-y-2 text-xs">
                    <div class="flex justify-between py-1 border-b border-cardborder/60">
                        <span class="text-mutedtext">Selected Duration:</span>
                        <strong id="summaryDays" class="text-white">1 Day(s)</strong>
                    </div>
                    <div class="flex justify-between py-1 border-b border-cardborder/60">
                        <span class="text-mutedtext">Applied Rate Structure:</span>
                        <strong id="summaryRatePlan" class="text-white">Daily Rate ($250.00/day)</strong>
                    </div>
                    <div class="flex justify-between py-1 border-b border-cardborder/60">
                        <span class="text-mutedtext">Base Rental Subtotal:</span>
                        <span id="summarySubtotal" class="font-mono text-white">$250.00</span>
                    </div>
                    <div class="flex justify-between py-1 border-b border-cardborder/60">
                        <span class="text-mutedtext">Georgia Sales Tax (7%):</span>
                        <span id="summaryTax" class="font-mono text-white">$17.50</span>
                    </div>
                    <div class="flex justify-between py-3 border-t border-cardborder text-base font-black">
                        <span class="text-white">Grand Total Due:</span>
                        <span id="summaryGrandTotal" class="text-kubota">$267.50</span>
                    </div>
                </div>
            </div>

            <!-- 6. Digital E-Signature Pad -->
            <div class="bg-cardbg p-6 rounded-2xl border border-cardborder shadow-xl">
                <h3 class="text-sm font-bold text-white mb-2 flex items-center gap-2">
                    <span class="bg-kubota text-white w-5 h-5 rounded-full inline-flex items-center justify-center text-xs font-black">6</span> 
                    Digital E-Signature
                </h3>
                <p class="text-xs text-mutedtext mb-3">Draw your signature below using your finger, stylus, or mouse to execute this binding contract.</p>

                <div class="border border-cardborder rounded-xl p-2 bg-darkbg relative">
                    <canvas id="sigCanvas" width="700" height="180" class="w-full h-44 touch-none cursor-crosshair rounded-lg bg-darkbg"></canvas>
                    <div id="sigPlaceholder" class="absolute inset-0 flex items-center justify-center pointer-events-none text-mutedtext text-xs italic">
                        Sign Here with Finger or Mouse
                    </div>
                </div>

                <div class="flex justify-between items-center mt-3">
                    <button type="button" id="clearSig" class="text-xs text-red-400 hover:text-red-300 font-medium flex items-center gap-1 px-3 py-1.5 rounded-lg border border-red-900/50 hover:bg-red-950/50 transition">
                        <i class="fa-solid fa-eraser"></i> Clear Signature
                    </button>
                    <span class="text-[10px] text-mutedtext">Timestamped upon submission</span>
                </div>
            </div>

            <!-- SUBMIT BUTTON -->
            <div class="no-print">
                <button type="submit" id="submitBtn" class="w-full bg-kubota hover:bg-kubotadark text-white font-black text-base py-4 rounded-xl shadow-xl transition duration-200 flex items-center justify-center gap-2">
                    <i class="fa-solid fa-file-signature"></i> EXECUTE RENTAL AGREEMENT & LOCK BOOKING
                </button>
                <p class="text-center text-[10px] text-mutedtext mt-2">By submitting, you agree to all terms and write dates to Osiris Grading & Rental database.</p>
            </div>

        </form>

        <!-- EXECUTED / CONFIRMATION RECEIPT VIEW (Hidden Initially) -->
        <div id="confirmationReceipt" class="hidden bg-cardbg p-8 rounded-2xl border border-cardborder shadow-2xl space-y-6">
            <div class="flex justify-between items-start border-b border-cardborder pb-4">
                <div>
                    <span class="bg-green-950 text-green-400 border border-green-800 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider">Agreement Executed & Active</span>
                    <h2 class="text-2xl font-black text-white mt-2">Osiris Grading & Rental Receipt</h2>
                    <p id="receiptID" class="text-xs text-mutedtext font-mono mt-1">ID: -</p>
                </div>
                <button onclick="window.print()" class="no-print bg-darkbg hover:bg-cardborder text-white text-xs px-4 py-2.5 rounded-lg font-bold flex items-center gap-2 border border-cardborder transition">
                    <i class="fa-solid fa-print"></i> Print / Save PDF
                </button>
            </div>

            <div class="grid grid-cols-2 gap-4 text-xs bg-darkbg p-4 rounded-xl border border-cardborder">
                <div>
                    <span class="text-mutedtext block">Renter Name:</span>
                    <strong id="rName" class="text-sm text-white">-</strong>
                </div>
                <div>
                    <span class="text-mutedtext block">Phone:</span>
                    <strong id="rPhone" class="text-sm text-white">-</strong>
                </div>
                <div>
                    <span class="text-mutedtext block">Driver's License:</span>
                    <strong id="rDL" class="text-sm text-white">-</strong>
                </div>
                <div>
                    <span class="text-mutedtext block">Rental Period:</span>
                    <strong id="rDates" class="text-sm text-kubota">-</strong>
                </div>
                <div>
                    <span class="text-mutedtext block">Start Engine Hours:</span>
                    <strong id="rHours" class="text-sm text-white">-</strong>
                </div>
                <div>
                    <span class="text-mutedtext block">Operator Status:</span>
                    <strong id="rOperator" class="text-sm text-white">Machine Only</strong>
                </div>
            </div>

            <!-- Financial Receipt Breakdown -->
            <div class="bg-darkbg p-4 rounded-xl border border-cardborder text-xs space-y-1.5">
                <div class="flex justify-between">
                    <span class="text-mutedtext">Rate Plan & Subtotal:</span>
                    <span id="rSubtotal" class="text-white font-mono">$0.00</span>
                </div>
                <div class="flex justify-between">
                    <span class="text-mutedtext">Georgia Sales Tax (7%):</span>
                    <span id="rTax" class="text-white font-mono">$0.00</span>
                </div>
                <div class="flex justify-between pt-2 border-t border-cardborder text-sm font-black">
                    <span class="text-white">Total Amount Paid:</span>
                    <span id="rGrandTotal" class="text-kubota">$0.00</span>
                </div>
            </div>

            <div>
                <h4 class="text-xs font-bold text-mutedtext mb-2">Executed Digital Signature:</h4>
                <div class="border border-cardborder rounded-xl p-2 bg-darkbg w-72">
                    <img id="rSigImg" src="" alt="Customer Signature" class="h-20 object-contain">
                </div>
                <p id="rTimestamp" class="text-[10px] text-mutedtext mt-1">Timestamp: -</p>
            </div>

            <div class="pt-4 border-t border-cardborder text-center text-xs text-mutedtext">
                <p>Thank you for choosing <strong class="text-white">Osiris Grading & Rental</strong>! For jobsite assistance, contact <strong>404-951-0578</strong>.</p>
            </div>
        </div>

    </main>

    <!-- APPLICATION LOGIC -->
    <script>
        // --- 1. FIREBASE SETUP & LOCAL FALLBACK ---
        const firebaseConfig = {
            apiKey: "AIzaSyDEMO_KEY_REPLACE_ME",
            authDomain: "osiris-grading-demo.firebaseapp.com",
            databaseURL: "https://osiris-grading-demo-default-rtdb.firebaseio.com",
            projectId: "osiris-grading-demo"
        };

        let db = null;
        let localBookings = [];

        try {
            firebase.initializeApp(firebaseConfig);
            db = firebase.database();
            document.getElementById('firebaseStatus').innerText = 'Firebase Live Sync';
            
            db.ref('rentals').on('value', (snapshot) => {
                localBookings = [];
                snapshot.forEach((childSnap) => {
                    const data = childSnap.val();
                    if (data.startDate && data.endDate) {
                        localBookings.push({ start: data.startDate, end: data.endDate });
                    }
                });
                renderCalendar();
            });
        } catch (e) {
            document.getElementById('firebaseStatus').innerText = 'Local Demo Mode';
            document.getElementById('firebaseStatus').className = 'text-[10px] bg-amber-950 text-amber-400 border border-amber-800 px-2 py-0.5 rounded-full font-medium';
        }

        // --- 2. PRICING & CALCULATION ENGINE ---
        const DAILY_RATE = 250.00;
        const WEEKLY_RATE = 875.00;
        const MONTHLY_RATE = 2500.00;
        const OPERATOR_DAILY_FEE = 450.00; // Total $700/day with operator
        const GA_TAX_RATE = 0.07;

        function calculateRentalPricing(startDateStr, endDateStr, includeOperator) {
            if (!startDateStr || !endDateStr) {
                const base = includeOperator ? 700.00 : DAILY_RATE;
                return { days: 1, ratePlan: includeOperator ? 'Machine + Certified Operator ($700.00/day)' : 'Daily Rate ($250.00/day)', subtotal: base, tax: base * GA_TAX_RATE, grandTotal: base * 1.07 };
            }

            const start = new Date(startDateStr);
            const end = new Date(endDateStr);
            const diffTime = Math.max(0, end - start);
            const days = Math.ceil(diffTime / (1000 * 60 * 60 * 24)) + 1;

            let subtotal = 0;
            let ratePlan = '';

            if (includeOperator) {
                // $700 per day total for Machine + Certified Operator
                subtotal = days * 700.00;
                ratePlan = `Machine + Certified Operator (${days} Days @ $700/day)`;
            } else {
                if (days >= 30) {
                    const months = Math.floor(days / 30);
                    const remDays = days % 30;
                    subtotal = (months * MONTHLY_RATE) + Math.min(remDays * DAILY_RATE, MONTHLY_RATE);
                    ratePlan = `Monthly Rate Plan (${days} Days)`;
                } else if (days >= 7) {
                    const weeks = Math.floor(days / 7);
                    const remDays = days % 7;
                    subtotal = (weeks * WEEKLY_RATE) + Math.min(remDays * DAILY_RATE, WEEKLY_RATE);
                    ratePlan = `Weekly Rate Plan (${days} Days)`;
                } else {
                    subtotal = Math.min(days * DAILY_RATE, WEEKLY_RATE);
                    ratePlan = days === 1 ? 'Daily Rate ($250.00/day)' : `Daily Rate (${days} Days @ $250/day)`;
                }
            }

            const tax = subtotal * GA_TAX_RATE;
            const grandTotal = subtotal + tax;

            return { days, ratePlan, subtotal, tax, grandTotal };
        }

        function updatePricingUI() {
            const startVal = document.getElementById('startDate').value;
            const endVal = document.getElementById('endDate').value;
            const includeOperator = document.getElementById('includeOperator').checked;
            const calc = calculateRentalPricing(startVal, endVal, includeOperator);

            document.getElementById('summaryDays').innerText = `${calc.days} Day(s)`;
            document.getElementById('summaryRatePlan').innerText = calc.ratePlan;
            document.getElementById('summarySubtotal').innerText = `$${calc.subtotal.toFixed(2)}`;
            document.getElementById('summaryTax').innerText = `$${calc.tax.toFixed(2)}`;
            document.getElementById('summaryGrandTotal').innerText = `$${calc.grandTotal.toFixed(2)}`;
        }

        document.getElementById('includeOperator').addEventListener('change', updatePricingUI);

        // --- 3. MULTI-DAY CALENDAR RANGE SELECTION ---
        let currentDate = new Date();
        let selectedStart = null;
        let selectedEnd = null;

        function renderCalendar() {
            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();

            const monthNames = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            document.getElementById('calendarMonthYear').innerText = `${monthNames[month]} ${year}`;

            const firstDay = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();

            const calendarDays = document.getElementById('calendarDays');
            calendarDays.innerHTML = '';

            for (let i = 0; i < firstDay; i++) {
                const empty = document.createElement('div');
                empty.className = 'p-2';
                calendarDays.appendChild(empty);
            }

            for (let day = 1; day <= daysInMonth; day++) {
                const dayDiv = document.createElement('div');
                const dateString = `${year}-${String(month + 1).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                
                dayDiv.innerText = day;
                dayDiv.className = 'p-2 rounded-lg border border-cardborder/40 calendar-day transition font-medium';

                const isBooked = localBookings.some(b => dateString >= b.start && dateString <= b.end);

                if (isBooked) {
                    dayDiv.classList.add('disabled');
                    dayDiv.title = 'Rented / Unavailable';
                } else {
                    if (selectedStart && dateString === selectedStart) {
                        dayDiv.classList.add('selected-endpoint');
                    } else if (selectedEnd && dateString === selectedEnd) {
                        dayDiv.classList.add('selected-endpoint');
                    } else if (selectedStart && selectedEnd && dateString > selectedStart && dateString < selectedEnd) {
                        dayDiv.classList.add('in-range');
                    } else {
                        dayDiv.classList.add('hover:bg-kubota/20', 'hover:border-kubota/50', 'cursor-pointer', 'text-white');
                    }

                    dayDiv.onclick = () => handleCalendarClick(dateString);
                }

                calendarDays.appendChild(dayDiv);
            }
        }

        function handleCalendarClick(dateStr) {
            if (!selectedStart || (selectedStart && selectedEnd)) {
                selectedStart = dateStr;
                selectedEnd = null;
                document.getElementById('startDate').value = dateStr;
                document.getElementById('endDate').value = dateStr;
            } else if (dateStr >= selectedStart) {
                const hasConflict = localBookings.some(b => b.start <= dateStr && b.end >= selectedStart);
                if (hasConflict) {
                    alert("Selected date range includes booked dates. Please pick available dates.");
                    return;
                }
                selectedEnd = dateStr;
                document.getElementById('endDate').value = dateStr;
            } else {
                selectedStart = dateStr;
                selectedEnd = null;
                document.getElementById('startDate').value = dateStr;
                document.getElementById('endDate').value = dateStr;
            }

            renderCalendar();
            updatePricingUI();
        }

        document.getElementById('startDate').addEventListener('change', (e) => {
            selectedStart = e.target.value;
            if (selectedEnd && selectedEnd < selectedStart) selectedEnd = selectedStart;
            renderCalendar();
            updatePricingUI();
        });

        document.getElementById('endDate').addEventListener('change', (e) => {
            selectedEnd = e.target.value;
            if (selectedStart && selectedStart > selectedEnd) selectedStart = selectedEnd;
            renderCalendar();
            updatePricingUI();
        });

        document.getElementById('prevMonth').onclick = () => { currentDate.setMonth(currentDate.getMonth() - 1); renderCalendar(); };
        document.getElementById('nextMonth').onclick = () => { currentDate.setMonth(currentDate.getMonth() + 1); renderCalendar(); };

        // Set default dates
        const todayStr = new Date().toISOString().split('T')[0];
        document.getElementById('startDate').value = todayStr;
        document.getElementById('endDate').value = todayStr;
        selectedStart = todayStr;
        selectedEnd = todayStr;

        renderCalendar();
        updatePricingUI();

        // --- 4. E-SIGNATURE CANVAS LOGIC ---
        const canvas = document.getElementById('sigCanvas');
        const ctx = canvas.getContext('2d');
        const placeholder = document.getElementById('sigPlaceholder');
        let isDrawing = false;
        let hasSigned = false;

        ctx.lineWidth = 2.5;
        ctx.lineCap = 'round';
        ctx.strokeStyle = '#F36C21';

        function getPos(e) {
            const rect = canvas.getBoundingClientRect();
            const clientX = e.touches ? e.touches[0].clientX : e.clientX;
            const clientY = e.touches ? e.touches[0].clientY : e.clientY;
            return { x: clientX - rect.left, y: clientY - rect.top };
        }

        function startDraw(e) {
            isDrawing = true;
            hasSigned = true;
            placeholder.style.display = 'none';
            const pos = getPos(e);
            ctx.beginPath();
            ctx.moveTo(pos.x, pos.y);
        }

        function draw(e) {
            if (!isDrawing) return;
            const pos = getPos(e);
            ctx.lineTo(pos.x, pos.y);
            ctx.stroke();
        }

        function stopDraw() { isDrawing = false; }

        canvas.addEventListener('mousedown', startDraw);
        canvas.addEventListener('mousemove', draw);
        canvas.addEventListener('mouseup', stopDraw);
        canvas.addEventListener('touchstart', (e) => { startDraw(e); e.preventDefault(); });
        canvas.addEventListener('touchmove', (e) => { draw(e); e.preventDefault(); });
        canvas.addEventListener('touchend', stopDraw);

        document.getElementById('clearSig').onclick = () => {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            placeholder.style.display = 'flex';
            hasSigned = false;
        };

        // --- 5. FORM SUBMISSION LOGIC ---
        document.getElementById('rentalForm').onsubmit = function(e) {
            e.preventDefault();

            if (!hasSigned) {
                alert("Please provide your digital signature on the pad before submitting.");
                return;
            }

            const startDate = document.getElementById('startDate').value;
            const endDate = document.getElementById('endDate').value;
            const includeOperator = document.getElementById('includeOperator').checked;

            if (endDate < startDate) {
                alert("Return Date cannot be earlier than Start Date.");
                return;
            }

            const calc = calculateRentalPricing(startDate, endDate, includeOperator);

            const rentalData = {
                rentalID: 'OGR-' + Math.floor(100000 + Math.random() * 900000),
                renterName: document.getElementById('renterName').value,
                phone: document.getElementById('renterPhone').value,
                dl: document.getElementById('renterDL').value,
                address: document.getElementById('renterAddress').value,
                startDate: startDate,
                endDate: endDate,
                startHours: document.getElementById('startHours').value,
                includeOperator: includeOperator,
                attachment: 'Standard Tooth Bucket',
                pricing: calc,
                signatureImg: canvas.toDataURL(),
                timestamp: new Date().toLocaleString()
            };

            if (db) {
                db.ref('rentals/' + rentalData.rentalID).set(rentalData);
            } else {
                localBookings.push({ start: startDate, end: endDate });
                renderCalendar();
            }

            document.getElementById('rentalForm').classList.add('hidden');
            document.getElementById('receiptID').innerText = `Agreement ID: ${rentalData.rentalID}`;
            document.getElementById('rName').innerText = rentalData.renterName;
            document.getElementById('rPhone').innerText = rentalData.phone;
            document.getElementById('rDL').innerText = rentalData.dl;
            document.getElementById('rDates').innerText = `${rentalData.startDate} to ${rentalData.endDate} (${calc.days} Day/s)`;
            document.getElementById('rHours').innerText = `${rentalData.startHours} Hours`;
            document.getElementById('rOperator').innerText = includeOperator ? 'Machine + Certified Operator Included' : 'Machine Only';
            document.getElementById('rSubtotal').innerText = `$${calc.subtotal.toFixed(2)}`;
            document.getElementById('rTax').innerText = `$${calc.tax.toFixed(2)}`;
            document.getElementById('rGrandTotal').innerText = `$${calc.grandTotal.toFixed(2)}`;
            document.getElementById('rSigImg').src = rentalData.signatureImg;
            document.getElementById('rTimestamp').innerText = `Executed on: ${rentalData.timestamp}`;

            document.getElementById('confirmationReceipt').classList.remove('hidden');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        };
    </script>
</body>
</html>
