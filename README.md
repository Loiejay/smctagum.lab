<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SMC Lab Chemical Locator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        /* Custom styles for better readability and a clean look */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* Light gray background */
        }
        .prose {
            max-width: 100%;
        }
        /* Ensure responsive height on larger screens */
        @media (min-width: 1024px) {
            #chemical-list-container {
                max-height: calc(100vh - 140px);
            }
        }
        /* Style for the static information panel */
        .info-panel h4 {
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            margin-bottom: 0.75rem;
            padding-bottom: 0.5rem;
            border-bottom: 1px solid #e2e8f0;
        }
    </style>
</head>
<body class="text-slate-800 flex flex-col min-h-screen">

    <!-- Header: Royal Blue Background -->
    <header class="bg-blue-900 text-white p-6 shadow-xl border-b-4 border-yellow-500">
        <div class="max-w-6xl mx-auto">
            <div class="flex flex-col items-center justify-center mb-6 border-b border-blue-700/50 pb-4">
                <!-- Logo/Placeholder -->
                <img 
                    src="https://smctagum.edu.ph/main/wp-content/uploads/2022/02/cropped-banner-2-01.png" 
                    alt="St. Mary's College of Tagum Inc. Banner" 
                    class="w-full max-w-sm h-auto rounded-lg object-contain mb-4 border-2 border-white shadow-xl bg-white p-2" 
                    onerror="this.onerror=null; this.src='https://placehold.co/400x100/FFFFFF/000000?text=SMC+TAGUM+LAB';"
                />
                <p class="text-xl font-semibold tracking-wider text-yellow-400">
                    St. Mary's College of Tagum Inc.
                </p>
            </div>
            
            <div class="flex flex-col md:flex-row justify-between items-center gap-4 mt-4">
                <div class="flex items-center gap-3">
                    <div class="bg-white p-2 rounded-full shadow-md">
                        <i data-lucide="beaker" class="w-8 h-8 text-blue-900"></i>
                    </div>
                    <div>
                        <h1 class="text-2xl font-bold tracking-wide">Lab Inventory Locator</h1>
                        <p class="text-yellow-400 text-sm font-medium">Royal Chemical Database (Static Version)</p>
                    </div>
                </div>
                <div class="relative w-full md:w-96">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <i data-lucide="search" class="h-5 w-5 text-blue-300"></i>
                    </div>
                    <input
                        type="text"
                        id="search-input"
                        class="block w-full pl-10 pr-3 py-2 border-2 border-blue-800 rounded-lg leading-5 bg-blue-800 text-white placeholder-blue-300 focus:outline-none focus:ring-2 focus:ring-yellow-400 focus:bg-blue-900 transition-colors shadow-inner"
                        placeholder="Search chemical name..."
                        oninput="handleSearch(this.value)"
                    />
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="flex-1 max-w-6xl mx-auto w-full p-4 md:p-6 grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Left Column: Search Results -->
        <div id="chemical-list-container" class="bg-white rounded-xl shadow-xl border border-gray-100 overflow-hidden flex flex-col h-[500px] lg:h-[calc(100vh-140px)]">
            <div class="p-4 border-b border-gray-100 bg-gray-50 flex justify-between items-center">
                <span class="font-bold text-blue-900 uppercase tracking-wider text-sm">Chemical List</span>
                <span id="result-count" class="text-xs font-bold px-3 py-1 bg-yellow-100 text-yellow-800 rounded-full border border-yellow-200">
                    208 Found
                </span>
            </div>
            <div id="chemical-list" class="overflow-y-auto flex-1 p-3 grid grid-cols-2 md:grid-cols-3 gap-2"> 
                <!-- Chemical buttons go here -->
            </div>
        </div>

        <!-- Right Column: Locator Map & Info -->
        <div class="lg:col-span-2 flex flex-col gap-6">
            
            <!-- Top Panel: Selection & Info -->
            <div class="bg-white rounded-xl shadow-xl border border-gray-100 p-6">
                <div class="mb-4 border-b border-gray-100 pb-4 flex justify-between items-start">
                    <div>
                        <h2 class="text-xl font-bold text-blue-900 flex items-center gap-2">
                            <i data-lucide="map-pin" class="text-yellow-500 fill-yellow-500 w-6 h-6"></i>
                            Locator & Static Guide
                        </h2>
                        <p class="text-sm text-gray-500 mt-1">Select a chemical to view location and static safety data</p>
                    </div>
                </div>

                <div id="selected-chemical-panel">
                    <!-- Dynamic content loads here -->
                    <div id="selection-placeholder" class="text-center py-10 bg-gray-50 rounded-lg border-2 border-dashed border-gray-200 text-gray-400">
                        <i data-lucide="search" class="mx-auto h-10 w-10 mb-3 opacity-30 text-blue-900"></i>
                        <p class="font-medium">Select a chemical from the list to see details</p>
                    </div>
                </div>
            </div>

            <!-- Bottom Panel: Cabinet Map -->
            <div class="bg-white rounded-xl shadow-xl border border-gray-100 p-6 flex-1 flex flex-col min-h-[400px]">
                <h3 class="font-bold text-blue-900 mb-6 uppercase tracking-wider text-sm flex items-center gap-2">
                    <i data-lucide="archive" class="text-yellow-500 w-4 h-4"></i> Visual Map
                </h3>
                <div class="flex-1 flex gap-4 sm:gap-8 justify-center items-start overflow-y-auto">
                    <!-- LEFT SIDE CABINET -->
                    <div class="flex flex-col items-center w-1/2">
                        <h3 class="mb-4 font-bold text-xs sm:text-sm text-gray-500 flex items-center gap-2">
                            <i data-lucide="arrow-left" class="w-3 h-3"></i> LEFT SIDE
                        </h3>
                        <div id="left-cabinet" class="w-full grid grid-cols-2 sm:grid-cols-3 gap-2 sm:gap-3 bg-gray-50 p-3 rounded-xl border border-gray-200">
                            <!-- Left shelves go here -->
                        </div>
                    </div>

                    <!-- DIVIDER -->
                    <div class="w-px bg-gray-200 h-full mx-1"></div>

                    <!-- RIGHT SIDE CABINET -->
                    <div class="flex flex-col items-center w-1/2">
                        <h3 class="mb-4 font-bold text-xs sm:text-sm text-gray-500 flex items-center gap-2">
                            RIGHT SIDE <i data-lucide="arrow-right" class="w-3 h-3"></i>
                        </h3>
                        <div id="right-cabinet" class="w-full grid grid-cols-2 sm:grid-cols-3 gap-2 sm:gap-3 bg-gray-50 p-3 rounded-xl border border-gray-200">
                            <!-- Right shelves go here -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <script>
        // --- DATA SECTION (CSV Data) ---
        const rawCsvData = `
CABINET -1 ,,,,,,SIDE ,,CUBILCE 
1. BROMOCRESOL GREEN INDICATOR ,,,,,,RIGHT ,,1
2.BROMOTHYMOL  BLUE ,,,,,,RIGHT ,,1
3.BROMOTHYMOL RED ,,,,,,RIGHT ,,1
4.PHENYLDRAZINE HYDROCHILORIDE ,,,,,,RIGHT ,,1
5.POTASSIUM PERMANGANATE ,,,,,,RIGHT ,,1
6.SODUIM BISULFATE ,,,,,,RIGHT ,,1
7.SUDIUM ACETATE ,,,,,,RIGHT ,,2
8.BETADINE ,,,,,,RIGHT ,,3
9.CRYSTAL VIOLET ,,,,,,RIGHT ,,3
10.IODINE ,,,,,,RIGHT ,,3
11.METHANE BLUE ,,,,,,RIGHT ,,3
12.SAFARANIN ,,,,,,RIGHT ,,3
13.ALCOHOLIC KOH ,,,,,,RIGHT ,,4
14.ISOPROPLY ALCOHOL ,,,,,,RIGHT ,,4
15.AMINO ACID SOLITION ,,,,,,RIGHT ,,5
16.GLUCOSE SOLUTION ,,,,,,RIGHT ,,5
17.PHENOLTHALIEN SOLUTION ,,,,,,RIGHT ,,5
18.RINGER'S SOLUTION,,,,,,RIGHT ,,5
19.ZINC NITRATE SOLITION ,,,,,,RIGHT ,,5
20.BENEDICT'S SOLUTION ,,,,,,RIGHT ,,6
21.LEAD NITRATE ,,,,,,RIGHT ,,7
22.NINHYDRIN,,,,,,RIGHT ,,7
23.STRONTIUM NITRATE,,,,,,RIGHT ,,7
24.ZINC NITRATE ,,,,,,RIGHT ,,7
25.AMMONIUM CHLORIDE,,,,,,RIGHT ,,8
26.CALCIUM CHLORIDE,,,,,,RIGHT ,,8
27.IRON(III) CHLORIDE,,,,,,RIGHT ,,8
28.MAGNESIUM CHLORIDE,,,,,,RIGHT ,,8
29.SODIUM CHLORIDE,,,,,,RIGHT ,,8
30.POTASSIUM CHLORIDE,,,,,,RIGHT ,,8
31. AMMONIUM THIOCYANATE ,,,,,,RIGHT ,,9
32. SODIUM THIOSULPHATE ,,,,,,RIGHT ,,9
33. SILVER NITRATE ,,,,,,RIGHT ,,9
34. IRON (II) SULFATE ,,,,,,RIGHT ,,9
35. POTASSIUM FERRICYANIDE ,,,,,,RIGHT ,,9
36. POTASSIUM FERROCYANIDE ,,,,,,RIGHT ,,9
37. MANGANESE DIOXIDE ,,,,,,RIGHT ,,9
38. HYDROGEN PEROXIDE ,,,,,,RIGHT ,,10
39. MAGNESIUM RIBBON ,,,,,,RIGHT ,,10
40. COPPER TURNINGS ,,,,,,RIGHT ,,10
41. SODIUM BICARBONATE ,,,,,,RIGHT ,,10
42. POTASSIUM IODATE ,,,,,,RIGHT ,,10
43. BARIUM CHLORIDE ,,,,,,RIGHT ,,11
44. CALCIUM CARBONATE ,,,,,,RIGHT ,,11
45. AMMONIUM PHOSPHATE ,,,,,,RIGHT ,,11
46. ZINC OXIDE ,,,,,,RIGHT ,,11
47. COPPER CARBONATE ,,,,,,RIGHT ,,11
48. ETHYL ALCOHOL ,,,,,,RIGHT ,,12
49. ETHYL ACETATE ,,,,,,RIGHT ,,12
50. ACETONE ,,,,,,RIGHT ,,12
51. HEXANE ,,,,,,RIGHT ,,12
52. BENZENE ,,,,,,RIGHT ,,12
53. AMYL ALCOHOL ,,,,,,RIGHT ,,12
54. DICHLOROMETHANE ,,,,,,RIGHT ,,13
55. DISSODIUM PHOSPHATE ,,,,,,RIGHT ,,13
56. SODIUM ACETATE ,,,,,,RIGHT ,,13
57. SODIUM HYDROGEN CARBONATE ,,,,,,RIGHT ,,13
58. GLYCEROL ,,,,,,RIGHT ,,14
59. LUGOL'S SOLUTION ,,,,,,RIGHT ,,14
60. GLUCOSE ,,,,,,RIGHT ,,14
61. Fructose ,,,,,,RIGHT ,,14
62. SUCROSE ,,,,,,RIGHT ,,14
63. DEXTROSE ,,,,,,RIGHT ,,14
64. LACTOSE ,,,,,,RIGHT ,,14
65. STARCH ,,,,,,RIGHT ,,14
66. POTASSIUM HYDROXIDE ,,,,,,RIGHT ,,15
67. SODIUM HYDROXIDE ,,,,,,RIGHT ,,15
68. CALCIUM HYDROXIDE ,,,,,,RIGHT ,,15
69. BISMUTH NITRATE ,,,,,,RIGHT ,,16
70. CADMIUM NITRATE ,,,,,,RIGHT ,,16
71. COBALT NITRATE ,,,,,,RIGHT ,,16
72. LEAD NITRATE ,,,,,,RIGHT ,,16
73. MANGANESE NITRATE ,,,,,,RIGHT ,,16
74. MERCURY (II) NITRATE ,,,,,,RIGHT ,,17
75. NICKEL NITRATE ,,,,,,RIGHT ,,17
76. ZINC NITRATE ,,,,,,RIGHT ,,17
77. COPPER NITRATE ,,,,,,RIGHT ,,17
78. ALUMINIUM NITRATE ,,,,,,RIGHT ,,17
79. AMMONIUM NITRATE ,,,,,,RIGHT ,,17
80. BARIUM NITRATE ,,,,,,RIGHT ,,18
81. IRON (III) NITRATE ,,,,,,RIGHT ,,18
82. MAGNESIUM NITRATE ,,,,,,RIGHT ,,18
83. POTASSIUM NITRATE ,,,,,,RIGHT ,,18
84. SODIUM NITRATE ,,,,,,RIGHT ,,18
85. SODIUM NITRITE ,,,,,,RIGHT ,,19
86. POTASSIUM NITRITE ,,,,,,RIGHT ,,19
87. HYDROCHLORIC ACID ,,,,,,RIGHT ,,19
88. SULFURIC ACID ,,,,,,RIGHT ,,19
89. NITRIC ACID ,,,,,,RIGHT ,,19
90. ACETIC ACID ,,,,,,RIGHT ,,19
91. METHANOL ,,,,,,RIGHT ,,20
92. PHENOL ,,,,,,RIGHT ,,20
93. BENZOIC ACID ,,,,,,RIGHT ,,20
94. CHLOROFORM ,,,,,,RIGHT ,,20
95. ETHYLENE GLYCOL ,,,,,,RIGHT ,,20
96. ETHYL ACETATE ,,,,,,RIGHT ,,20
97. METHYLENE BLUE ,,,,,,LEFT ,,1
98. NEUTRAL RED ,,,,,,LEFT ,,1
99. BROMOPHENOL BLUE ,,,,,,LEFT ,,1
100. CONGO RED ,,,,,,LEFT ,,1
101. ERYTHROSIN ,,,,,,LEFT ,,2
102. THYMOL BLUE ,,,,,,LEFT ,,2
103. CALMAGITE ,,,,,,LEFT ,,2
104. FERRIC CHLORIDE ,,,,,,LEFT ,,2
105. MAGNESIUM TURNINGS ,,,,,,LEFT ,,3
106. ALUMINIUM POWDER ,,,,,,LEFT ,,3
107. IRON POWDER ,,,,,,LEFT ,,3
108. ZINC POWDER ,,,,,,LEFT ,,3
109. CARBON POWDER ,,,,,,LEFT ,,3
110. POTASSIUM IODIDE ,,,,,,LEFT ,,4
111. SODIUM IODIDE ,,,,,,LEFT ,,4
112. SILVER IODIDE ,,,,,,LEFT ,,4
113. CALCIUM IODIDE ,,,,,,LEFT ,,4
114. BROMINE WATER ,,,,,,LEFT ,,5
115. CHLORINE WATER ,,,,,,LEFT ,,5
116. IODINE SOLUTION ,,,,,,LEFT ,,5
117. AMMONIUM OXALATE ,,,,,,LEFT ,,5
118. POTASSIUM OXALATE ,,,,,,LEFT ,,6
119. SODIUM OXALATE ,,,,,,LEFT ,,6
120. IRON (III) OXALATE ,,,,,,LEFT ,,6
121. CALCIUM OXALATE ,,,,,,LEFT ,,6
122. COPPER (II) OXALATE ,,,,,,LEFT ,,7
123. GLYCOGEN ,,,,,,LEFT ,,7
124. AGAR AGAR ,,,,,,LEFT ,,7
125. CELLULOSE ,,,,,,LEFT ,,7
126. GELATIN ,,,,,,LEFT ,,7
127. CASEIN ,,,,,,LEFT ,,7
128. AMYLACETATE ,,,,,,LEFT ,,8
129. BUTYL ACETATE ,,,,,,LEFT ,,8
130. ISOAMYL ACETATE ,,,,,,LEFT ,,8
131. ETHYL FORMATE ,,,,,,LEFT ,,8
132. METHYL ACETATE ,,,,,,LEFT ,,8
133. ACETYL CHLORIDE ,,,,,,LEFT ,,9
134. SODIUM CARBONATE ,,,,,,LEFT ,,9
135. POTASSIUM CARBONATE ,,,,,,LEFT ,,9
136. CALCIUM CARBONATE ,,,,,,LEFT ,,9
137. MAGNESIUM CARBONATE ,,,,,,LEFT ,,10
138. STRONTIUM CARBONATE ,,,,,,LEFT ,,10
139. LITHIUM CARBONATE ,,,,,,LEFT ,,10
140. LEAD CARBONATE ,,,,,,LEFT ,,10
141. IRON (II) CARBONATE ,,,,,,LEFT ,,10
142. BISMUTH CARBONATE ,,,,,,LEFT ,,11
143. CADMIUM CARBONATE ,,,,,,LEFT ,,11
144. COBALT CARBONATE ,,,,,,LEFT ,,11
145. NICKEL CARBONATE ,,,,,,LEFT ,,11
146. ZINC CARBONATE ,,,,,,LEFT ,,11
147. COPPER CARBONATE ,,,,,,LEFT ,,12
148. ALUMINIUM CARBONATE ,,,,,,LEFT ,,12
149. AMMONIUM CARBONATE ,,,,,,LEFT ,,12
150. BARIUM CARBONATE ,,,,,,LEFT ,,12
151. CHROMIUM (III) CARBONATE ,,,,,,LEFT ,,12
152. DYES MIXTURE ,,,,,,LEFT ,,13
153. PHENOL RED ,,,,,,LEFT ,,13
154. CRESOL RED ,,,,,,LEFT ,,13
155. METHYL ORANGE ,,,,,,LEFT ,,13
156. METHYL RED ,,,,,,LEFT ,,13
157. BROMOCRESOL PURPLE ,,,,,,LEFT ,,13
158. BROMOTHYMOL BLUE ,,,,,,LEFT ,,14
159. INDIGO CARMINE ,,,,,,LEFT ,,14
160. PICRIC ACID ,,,,,,LEFT ,,14
161. AMMONIUM FERRIC SULFATE ,,,,,,LEFT ,,14
162. POTASSIUM ALUMINIUM SULFATE ,,,,,,LEFT ,,14
163. ALUMINIUM SULFATE ,,,,,,LEFT ,,14
164. NICKEL SULFATE ,,,,,,LEFT ,,14
165. COPPER SULFATE ,,,,,,LEFT ,,15
166. IRON (II) SULFATE ,,,,,,LEFT ,,15
167. ZINC SULFATE ,,,,,,LEFT ,,15
168. CALCIUM SULFATE ,,,,,,LEFT ,,15
169. AMMONIUM SULFATE ,,,,,,LEFT ,,16
170. BARIUM SULFATE ,,,,,,LEFT ,,16
171. MAGNESIUM SULFATE ,,,,,,LEFT ,,16
172. POTASSIUM SULFATE ,,,,,,LEFT ,,16
173. SODIUM SULFATE ,,,,,,LEFT ,,16
174. STRONTIUM SULFATE ,,,,,,LEFT ,,17
175. LITHIUM SULFATE ,,,,,,LEFT ,,17
176. LEAD SULFATE ,,,,,,LEFT ,,17
177. IRON (III) SULFATE ,,,,,,LEFT ,,17
178. BISMUTH SULFATE ,,,,,,LEFT ,,17
179. CADMIUM SULFATE ,,,,,,LEFT ,,17
180. COBALT SULFATE ,,,,,,LEFT ,,18
181. MANGANESE SULFATE ,,,,,,LEFT ,,18
182. CHROMIUM (III) SULFATE ,,,,,,LEFT ,,18
183. POTASSIUM BROMIDE ,,,,,,LEFT ,,18
184. SODIUM BROMIDE ,,,,,,LEFT ,,18
185. AMMONIUM BROMIDE ,,,,,,LEFT ,,19
186. AMMIUM SULFATE,,,,,,LEFT,,19
187. BARIUM SULFATE,,,,,,LEFT,,19
188. CALCIUM CARBIDE ,,,,,,LEFT,,19
189. COPPER SULFATE,,,,,,LEFT,,20
190. COPPER SULFATE ,,,,,,LEFT,,20
191. MANGANESE SULAFATE,,,,,,LEFT,,20
192. MANGANESE (II) SULFATE,,,,,,LEFT,,20
193. MAGNESIUM SULFATE, HYDRATED ,,,,,,LEFT,,20
194. POTASSIUM BISULFATE,,,,,,LEFT,,21
195. POTASSIUM SULFATE,,,,,,LEFT,,21
196. SODIUM METABISULFATE,,,,,,LEFT,,21
197. SODIUM SULFATE,,,,,,LEFT,,21
198. STATE POTATO,,,,,,LEFT,,22
199. ZINC SULAFATE,,,,,,LEFT,,22
200. BUFFERED FORMALIN,,,,,,LEFT,,22
201. DENATURED ALCOHOL,,,,,,LEFT,,23
202. ETHYL ALCOHOL,,,,,,LEFT,,23
203. ISOPROPYL ALCOHOL,,,,,,LEFT,,23
204. AMMONIUM BROMIDE,,,,,,LEFT,,24
205. AMMONIUM DIHYDROGEN ORTHOPHOSPHATE,,,,,,LEFT,,24
206. AMMONIUM IODIDE,,,,,,LEFT,,24
207. CALCIUM PHOSPHATE,,,,,,LEFT,,24
208. MALTOSE MONOHYDRATE 92%,,,,,,LEFT,,24
        `;

        // Global variables for application state
        let allChemicals = [];
        let filteredChemicals = [];
        let selectedChemical = null;
        let shelfNumbers = { left: [], right: [] };

        // --- DATA PARSING AND PREPARATION ---
        function parseData(data) {
            const lines = data.split('\n');
            const parsed = [];

            lines.forEach((line) => {
                const parts = line.split(',');
                if (parts.length < 5) return;
                let rawName = parts[0] || "";
                if (!rawName || rawName.includes("CABINET")) return;

                // Clean up the name: remove leading number/dot/space and trim
                const name = rawName.replace(/^\d+\.?\s*/, '').trim();
                
                // Attempt to find SIDE ('LEFT' or 'RIGHT')
                const sideIndex = parts.findIndex(p => p && (p.trim() === 'LEFT' || p.trim() === 'RIGHT'));
                if (sideIndex === -1) return;
                const side = parts[sideIndex].trim();
                
                // Attempt to find CUBICLE
                let cubicle = null;
                let cubicleCandidate = parts[sideIndex + 2] || parts[parts.length - 1] || parts[sideIndex + 1];

                if (cubicleCandidate) {
                    cubicle = parseInt(cubicleCandidate.trim());
                }

                if (name && side && !isNaN(cubicle)) {
                    parsed.push({
                        id: parsed.length + 1,
                        name: name,
                        side: side,
                        cubicle: cubicle
                    });
                }
            });
            return parsed;
        }

        function calculateShelves(chemicals) {
            const leftShelves = new Set();
            const rightShelves = new Set();
            chemicals.forEach(chem => {
                if (chem.side === 'LEFT') leftShelves.add(chem.cubicle);
                if (chem.side === 'RIGHT') rightShelves.add(chem.cubicle);
            });
            
            shelfNumbers = {
                left: Array.from(leftShelves).sort((a, b) => a - b),
                right: Array.from(rightShelves).sort((a, b) => a - b)
            };
        }

        // --- STATIC CONTENT REPLACING GEMINI ---
        function getStaticSafetyData(chemicalName) {
            chemicalName = chemicalName.toLowerCase();
            
            let type = "General Chemical";
            let data = {
                safety: "Consult the Safety Data Sheet (SDS). Wear standard lab coat, gloves, and eye protection. Avoid inhalation and ingestion.",
                disposal: "Collect all waste in a properly labeled non-hazardous waste container. Do not pour down the sink unless specifically approved by lab management.",
                uses: "Often used in quantitative analysis or synthesis experiments. Verify concentration before use."
            };

            // Classification Logic for static data
            if (chemicalName.includes('acid') && !chemicalName.includes('amino')) {
                type = "Strong Acid/Corrosive";
                data.safety = "<h4>**Hazards**</h4><p class='mt-2'>Highly Corrosive; causes severe chemical burns. May react violently with bases or water. Always store below eye level.</p><h4>**PPE**</h4><p class='mt-2'>Neoprene or rubber gloves, full wrap-around safety goggles or face shield, and chemical-resistant lab coat. Use in a fume hood.</p><h4>**Handling**</h4><p class='mt-2'>**ALWAYS ADD ACID TO WATER (AAA)**. Never the reverse. Pour slowly and stir constantly to dissipate heat.</p>";
                data.disposal = "<h4>**Pre-Disposal Check**</h4><p class='mt-2'>Must be neutralized to a pH between 6 and 8. Always use a pH meter or test paper to confirm the neutralization.</p><h4>**Procedure**</h4><p class='mt-2'>Slowly neutralize the acid waste with a dilute sodium bicarbonate (baking soda) or sodium hydroxide solution. Stir well and monitor temperature. Once neutralized, flush down the drain with large amounts of running water.</p>";
                data.uses = "Titrations, making standard solutions, dissolving metal oxides, and chemical synthesis.";
            } else if (chemicalName.includes('hydroxide') || chemicalName.includes('koh') || chemicalName.includes('alkali')) {
                type = "Strong Alkali (Base)/Corrosive";
                data.safety = "<h4>**Hazards**</h4><p class='mt-2'>Highly Corrosive; causes severe burns, especially to eyes. Solid pellets/powders generate extreme heat when dissolved in water.</p><h4>**PPE**</h4><p class='mt-2'>Rubber gloves, tight-fitting safety goggles, and a lab coat. Handle solids with a scoop, never fingers.</p><h4>**Handling**</h4><p class='mt-2'>Dissolve solids slowly in cold water while stirring in a sink, or in an ice bath, to control the exothermic reaction.</p>";
                data.disposal = "<h4>**Pre-Disposal Check**</h4><p class='mt-2'>Must be neutralized to a pH between 6 and 8. Use dilute acetic acid (vinegar) or hydrochloric acid as the neutralizer.</p><h4>**Procedure**</h4><p class='mt-2'>Slowly add base waste to the neutralizer solution while stirring. Monitor pH and temperature. Once neutralized, flush down the drain with large amounts of running water.</p>";
                data.uses = "Acid-base titrations, saponification (soap making), and cleaning organic residues from glassware.";
            } else if (chemicalName.includes('alcohol') || chemicalName.includes('acetone') || chemicalName.includes('hexane') || chemicalName.includes('chloroform')) {
                type = "Flammable/Volatile Solvent";
                data.safety = "<h4>**Hazards**</h4><p class='mt-2'>Highly Flammable (low flash point) and Volatile. Vapors can accumulate and ignite easily. Toxic if inhaled.</p><h4>**PPE**</h4><p class='mt-2'>Nitrile gloves, safety goggles, and lab coat. Ensure good ventilation.</p><h4>**Handling**</h4><p class='mt-2'>Keep away from ALL ignition sources (flames, sparks, hot plates). Work in a well-ventilated area or fume hood.</p>";
                data.disposal = "<h4>**Pre-Disposal Check**</h4><p class='mt-2'>DO NOT pour down the drain. Ensure the waste container is labeled with the exact contents (e.g., 'Waste Isopropyl Alcohol').</p><h4>**Procedure**</h4><p class='mt-2'>Collect all non-halogenated spent solvent in a designated 'Flammable Liquid Waste' container. Seal tightly and store in the flammables cabinet for hazardous waste collection.</p>";
                data.uses = "Extraction, recrystallization, chromatography, and as a general solvent for non-polar substances.";
            } else if (chemicalName.includes('nitrate') || chemicalName.includes('sulfate') || chemicalName.includes('chloride')) {
                type = "Inorganic Salt (General)";
                data.safety = "<h4>**Hazards**</h4><p class='mt-2'>Low immediate hazard, but toxic if large amounts are ingested. Some heavy metal salts (Lead, Mercury) are highly toxic.</p><h4>**PPE**</h4><p class='mt-2'>Standard lab coat and eye protection. Use gloves when handling powders to avoid skin exposure.</p><h4>**Handling**</h4><p class='mt-2'>Avoid generating dust. Store in a cool, dry area. Wash hands after handling.</p>";
                data.disposal = "<h4>**Pre-Disposal Check**</h4><p class='mt-2'>Check institutional guidelines. Most non-heavy metal salts (Na, K, Ca, Mg) are safe for sink disposal.</p><h4>**Procedure**</h4><p class='mt-2'>If non-regulated (e.g., NaCl, MgSO4), dissolve in excess water and flush down the drain with continuous running water. If containing heavy metals (e.g., Lead Nitrate), collect as hazardous waste.</p>";
                data.uses = "Precipitation reactions, solution preparation, qualitative analysis (testing for ions).";
            }
            
            // Final Disclaimer
            const disclaimer = `<div class="mt-4 text-xs italic text-gray-500 border-t pt-2">*Note: This is generic safety guidance for a ${type} chemical. Always follow your institution's official Safety Data Sheet (SDS) and Standard Operating Procedures (SOPs) for the chemical in your lab.*</div>`;
            data.safety += disclaimer;
            data.disposal += disclaimer;
            data.uses += disclaimer;

            return data;
        }


        // --- RENDERING FUNCTIONS ---

        function renderChemicalList(chemicals) {
            const listContainer = document.getElementById('chemical-list');
            const resultCount = document.getElementById('result-count');
            
            listContainer.innerHTML = ''; // Clear previous results
            resultCount.textContent = `${chemicals.length} Found`;

            if (chemicals.length === 0) {
                listContainer.innerHTML = `
                    <div class="col-span-full text-center p-10 text-gray-400">
                        <p>No chemicals found matching the search term.</p>
                    </div>
                `;
                return;
            }

            chemicals.forEach(chem => {
                const isSelected = selectedChemical && selectedChemical.id === chem.id;
                const buttonClass = `w-full text-center p-2 rounded-lg border transition-all duration-200 flex flex-col items-center justify-center group h-24
                    ${isSelected 
                        ? 'bg-blue-900 border-yellow-500 text-white shadow-md ring-1 ring-yellow-500' 
                        : 'bg-white border-gray-200 hover:bg-blue-50 hover:border-blue-300'
                    }`;
                const detailClass = `mt-1 flex items-center gap-1 text-[10px] px-2 py-0.5 rounded font-bold
                    ${isSelected ? 'bg-yellow-500 text-blue-900' : 'bg-gray-100 text-gray-500 group-hover:bg-white'}`;

                const button = document.createElement('button');
                button.className = buttonClass;
                button.innerHTML = `
                    <span class="text-sm font-semibold truncate w-full px-1">${chem.name}</span>
                    <div class="${detailClass}">CUB. ${chem.cubicle}</div>
                `;
                button.onclick = () => handleSelectChemical(chem);
                listContainer.appendChild(button);
            });
        }

        function renderCabinetMap() {
            const leftCabinet = document.getElementById('left-cabinet');
            const rightCabinet = document.getElementById('right-cabinet');
            
            leftCabinet.innerHTML = '';
            rightCabinet.innerHTML = '';

            // Render Left Cabinet Shelves
            shelfNumbers.left.forEach(shelfNum => {
                const isTarget = selectedChemical && selectedChemical.side === 'LEFT' && selectedChemical.cubicle === shelfNum;
                const shelfDiv = document.createElement('div');
                shelfDiv.className = `aspect-square rounded-lg flex flex-col items-center justify-center border-2 transition-all duration-300
                    ${isTarget 
                        ? 'bg-yellow-400 border-yellow-600 text-blue-900 shadow-xl scale-110 z-10' 
                        : 'bg-white border-gray-200 text-gray-300'
                    }`;
                shelfDiv.innerHTML = `
                    <i data-lucide="archive" class="${isTarget ? 'w-6 h-6' : 'w-4 h-4'}"></i>
                    <span class="text-xs sm:text-sm font-bold mt-1 ${isTarget ? 'text-blue-900' : 'text-gray-400'}">
                        ${shelfNum}
                    </span>
                `;
                leftCabinet.appendChild(shelfDiv);
            });

            // Render Right Cabinet Shelves
            shelfNumbers.right.forEach(shelfNum => {
                const isTarget = selectedChemical && selectedChemical.side === 'RIGHT' && selectedChemical.cubicle === shelfNum;
                const shelfDiv = document.createElement('div');
                shelfDiv.className = `aspect-square rounded-lg flex flex-col items-center justify-center border-2 transition-all duration-300
                    ${isTarget 
                        ? 'bg-yellow-400 border-yellow-600 text-blue-900 shadow-xl scale-110 z-10' 
                        : 'bg-white border-gray-200 text-gray-300'
                    }`;
                shelfDiv.innerHTML = `
                    <i data-lucide="archive" class="${isTarget ? 'w-6 h-6' : 'w-4 h-4'}"></i>
                    <span class="text-xs sm:text-sm font-bold mt-1 ${isTarget ? 'text-blue-900' : 'text-gray-400'}">
                        ${shelfNum}
                    </span>
                `;
                rightCabinet.appendChild(shelfDiv);
            });
            
            // Re-render Lucide icons
            lucide.createIcons();
        }

        function renderSelectedChemicalPanel() {
            const panel = document.getElementById('selected-chemical-panel');
            
            if (!selectedChemical) {
                // Show placeholder
                panel.innerHTML = `
                    <div id="selection-placeholder" class="text-center py-10 bg-gray-50 rounded-lg border-2 border-dashed border-gray-200 text-gray-400">
                        <i data-lucide="search" class="mx-auto h-10 w-10 mb-3 opacity-30 text-blue-900"></i>
                        <p class="font-medium">Select a chemical from the list to see details</p>
                    </div>
                `;
                return;
            }

            const staticData = getStaticSafetyData(selectedChemical.name);

            panel.innerHTML = `
                <div class="flex flex-col gap-4">
                    <!-- Location Banner -->
                    <div class="bg-gradient-to-r from-blue-50 to-white border-l-4 border-yellow-500 text-blue-900 p-4 rounded-r-lg shadow-sm flex flex-col sm:flex-row items-center justify-between gap-4">
                        <div class="flex items-center gap-4">
                            <div class="bg-blue-900 text-white p-3 rounded-full shadow-md border-2 border-yellow-400">
                                <i data-lucide="archive" class="w-6 h-6"></i>
                            </div>
                            <div>
                                <span class="block font-bold text-xl leading-tight text-blue-900">${selectedChemical.name}</span>
                                <span class="text-sm text-blue-700">
                                    Located in: <strong class="text-blue-900 bg-yellow-200 px-1 rounded">${selectedChemical.side} Side</strong>, Cubicle <strong class="text-blue-900 bg-yellow-200 px-1 rounded">${selectedChemical.cubicle}</strong>
                                </span>
                            </div>
                        </div>
                    </div>

                    <!-- Static Information Tabs -->
                    <div id="info-tabs" class="flex gap-2 p-1 bg-gray-100 rounded-lg">
                        <button class="tab-button w-1/3 p-2 rounded-lg text-sm font-semibold transition-colors bg-blue-900 text-white shadow-md" data-tab="safety">
                            <i data-lucide="shield-alert" class="w-4 h-4 inline mr-1"></i> Safety Snapshot
                        </button>
                        <button class="tab-button w-1/3 p-2 rounded-lg text-sm font-semibold transition-colors text-blue-900 hover:bg-white" data-tab="uses">
                            <i data-lucide="flask-conical" class="w-4 h-4 inline mr-1"></i> Suggested Uses
                        </button>
                        <button class="tab-button w-1/3 p-2 rounded-lg text-sm font-semibold transition-colors text-blue-900 hover:bg-white" data-tab="disposal">
                            <i data-lucide="trash-2" class="w-4 h-4 inline mr-1"></i> Disposal Guide
                        </button>
                    </div>

                    <!-- Static Content Area -->
                    <div id="info-content" class="info-panel mt-2 bg-gray-50 rounded-lg p-5 border border-gray-200 relative shadow-inner">
                        <!-- Default to Safety Tab Content -->
                        <div id="content-safety" class="prose prose-sm max-w-none text-slate-700 leading-relaxed">
                            ${staticData.safety}
                        </div>
                        <div id="content-uses" class="prose prose-sm max-w-none text-slate-700 leading-relaxed hidden">
                            ${staticData.uses}
                        </div>
                        <div id="content-disposal" class="prose prose-sm max-w-none text-slate-700 leading-relaxed hidden">
                            ${staticData.disposal}
                        </div>
                    </div>
                </div>
            `;
            
            // Setup Tab Switching Logic
            setupTabs();
            // Re-render Lucide icons
            lucide.createIcons();
        }
        
        function setupTabs() {
            const tabButtons = document.querySelectorAll('.tab-button');
            const infoContent = document.getElementById('info-content');

            tabButtons.forEach(button => {
                button.onclick = function() {
                    const targetTab = this.getAttribute('data-tab');

                    // Reset all buttons
                    tabButtons.forEach(btn => {
                        btn.className = 'tab-button w-1/3 p-2 rounded-lg text-sm font-semibold transition-colors text-blue-900 hover:bg-white';
                    });
                    
                    // Activate clicked button
                    this.className = 'tab-button w-1/3 p-2 rounded-lg text-sm font-semibold transition-colors bg-blue-900 text-white shadow-md';

                    // Hide all content and show target content
                    document.getElementById('content-safety').classList.add('hidden');
                    document.getElementById('content-uses').classList.add('hidden');
                    document.getElementById('content-disposal').classList.add('hidden');
                    
                    document.getElementById(`content-${targetTab}`).classList.remove('hidden');
                };
            });
        }

        // --- HANDLERS ---

        function handleSearch(searchTerm) {
            selectedChemical = null; // Deselect when searching
            const term = searchTerm.toLowerCase().trim();
            
            if (!term) {
                filteredChemicals = allChemicals;
            } else {
                filteredChemicals = allChemicals.filter(chem => 
                    chem.name.toLowerCase().includes(term)
                );
            }
            
            renderChemicalList(filteredChemicals);
            renderSelectedChemicalPanel(); // Update panel to show placeholder or new selection
            renderCabinetMap(); // Clear map highlight
        }

        function handleSelectChemical(chemical) {
            selectedChemical = chemical;
            
            // Re-render all parts that depend on the selection
            renderChemicalList(filteredChemicals); // Update button style
            renderCabinetMap(); // Update map highlight
            renderSelectedChemicalPanel(); // Update info panel
        }

        // --- INITIALIZATION ---
        function initApp() {
            allChemicals = parseData(rawCsvData);
            filteredChemicals = allChemicals;
            calculateShelves(allChemicals);
            
            // Initial render
            renderChemicalList(filteredChemicals);
            renderCabinetMap();
            renderSelectedChemicalPanel();
            
            // Create Lucide icons after all HTML content is loaded
            lucide.createIcons();
        }

        // Start the application after the document structure is loaded
        window.onload = initApp;
    </script>
</body>
</html>
