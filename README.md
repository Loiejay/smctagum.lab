import React, { useState, useEffect, useMemo } from 'react';
import { Search, MapPin, Beaker, Archive, ArrowRight, ArrowLeft, Sparkles, ShieldAlert, FlaskConical, Loader2 } from 'lucide-react';

// --- DATA SECTION ---
// NOTE: This list now contains the complete range of chemicals from 1 to 208, 
// including the previously missing block from 31 to 185, integrated from the uploaded file.
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

// --- GEMINI API HELPERS ---
const apiKey = ""; // The environment will provide this key at runtime

// Using exponential backoff for API calls
const callGemini = async (prompt, retries = 3) => {
  for (let i = 0; i < retries; i++) {
    try {
      const response = await fetch(
        `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`,
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            contents: [{ parts: [{ text: prompt }] }],
          }),
        }
      );

      if (response.status === 429 && i < retries - 1) {
        // Handle rate limiting with exponential backoff
        const delay = Math.pow(2, i) * 1000;
        await new Promise(resolve => setTimeout(resolve, delay));
        continue; // Retry the request
      }

      if (!response.ok) {
        throw new Error(`API Error: ${response.status}`);
      }

      const data = await response.json();
      return data.candidates?.[0]?.content?.parts?.[0]?.text || "No response generated.";

    } catch (error) {
      if (i === retries - 1) {
        console.error("Gemini API Error after retries:", error);
        return "Sorry, I couldn't fetch that information right now. Please try again.";
      }
      // Continue to next retry
    }
  }
};

const parseData = (data) => {
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
    
    // Attempt to find CUBICLE, which is usually 2 or 3 positions after SIDE
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
};

export default function App() {
  const [chemicals, setChemicals] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedChemical, setSelectedChemical] = useState(null);
  
  // AI State
  const [aiContent, setAiContent] = useState(null);
  const [isAiLoading, setIsAiLoading] = useState(false);
  const [aiMode, setAiMode] = useState(null); 

  useEffect(() => {
    const data = parseData(rawCsvData);
    setChemicals(data);
  }, []);

  const filteredChemicals = useMemo(() => {
    if (!searchTerm) return chemicals;
    return chemicals.filter(chem => 
      chem.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [chemicals, searchTerm]);

  const shelves = useMemo(() => {
    const leftShelves = new Set();
    const rightShelves = new Set();
    chemicals.forEach(chem => {
      if (chem.side === 'LEFT') leftShelves.add(chem.cubicle);
      if (chem.side === 'RIGHT') rightShelves.add(chem.cubicle);
    });
    return {
      // Sort numerically to ensure the map displays correctly
      left: Array.from(leftShelves).sort((a,b) => a - b),
      right: Array.from(rightShelves).sort((a,b) => a - b)
    };
  }, [chemicals]);

  // Reset AI content when selecting a new chemical
  const handleSelectChemical = (chem) => {
    setSelectedChemical(chem);
    setAiContent(null);
    setAiMode(null);
  };

  const handleGetSafetyInfo = async () => {
    if (!selectedChemical) return;
    setIsAiLoading(true);
    setAiMode('safety');
    setAiContent(null);
    
    const prompt = `You are a laboratory safety expert. Provide a brief, concise safety snapshot for the chemical "${selectedChemical.name}". 
    Format the response with these exact sections in bold:
    - **Hazards**: (Briefly list main hazards like Flammable, Corrosive, Toxic)
    - **PPE**: (Required protective gear)
    - **Handling**: (One key handling or storage tip)
    Keep the tone professional and serious. Max 100 words.`;

    const text = await callGemini(prompt);
    setAiContent(text);
    setIsAiLoading(false);
  };

  const handleGetUses = async () => {
    if (!selectedChemical) return;
    setIsAiLoading(true);
    setAiMode('uses');
    setAiContent(null);

    const prompt = `You are a science educator. List 3 interesting or common educational experiments/uses for the chemical "${selectedChemical.name}" in a school laboratory setting.
    Use bullet points. Keep it engaging but safe. Max 100 words.`;

    const text = await callGemini(prompt);
    setAiContent(text);
    setIsAiLoading(false);
  };

  return (
    <div className="min-h-screen bg-white font-sans text-slate-800 flex flex-col">
      {/* Header: Royal Blue Background */}
      <header className="bg-blue-900 text-white p-6 shadow-lg border-b-4 border-yellow-500">
        <div className="max-w-6xl mx-auto">
          {/* New: School Logo and Name (Updated for Banner Image) */}
          <div className="flex flex-col items-center justify-center mb-6 border-b border-blue-700/50 pb-4">
            {/* Logo for St. Mary's College of Tagum Inc. (Updated to Banner URL and smaller size) */}
            <img 
              src="https://smctagum.edu.ph/main/wp-content/uploads/2022/02/cropped-banner-2-01.png" 
              alt="St. Mary's College of Tagum Inc. Banner" 
              // Updated to max-w-sm
              className="w-full max-w-sm h-auto rounded-lg object-contain mb-4 border-2 border-white shadow-xl bg-white p-2" 
              onError={(e) => {
                e.target.onerror = null; // prevents infinite loop
                e.target.src = "https://placehold.co/400x100/FFFFFF/000000?text=SMC+TAGUM+LAB"; // Fallback to placeholder
              }}
            />
            <p className="text-xl font-semibold tracking-wider text-yellow-400">
              St. Mary's College of Tagum Inc.
            </p>
          </div>
          
          {/* Existing: App Title and Search Bar */}
          <div className="flex flex-col md:flex-row justify-between items-center gap-4 mt-4">
            <div className="flex items-center gap-3">
              <div className="bg-white p-2 rounded-full shadow-md">
                <Beaker className="w-8 h-8 text-blue-900" />
              </div>
              <div>
                <h1 className="text-2xl font-bold tracking-wide">Lab Inventory Locator</h1>
                <p className="text-yellow-400 text-sm font-medium">Royal Chemical Database</p>
              </div>
            </div>
            <div className="relative w-full md:w-96">
              <div className="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                <Search className="h-5 w-5 text-blue-300" />
              </div>
              <input
                type="text"
                className="block w-full pl-10 pr-3 py-2 border-2 border-blue-800 rounded-lg leading-5 bg-blue-800 text-white placeholder-blue-300 focus:outline-none focus:ring-2 focus:ring-yellow-400 focus:bg-blue-900 transition-colors shadow-inner"
                placeholder="Search chemical name..."
                value={searchTerm}
                onChange={(e) => {
                  setSearchTerm(e.target.value);
                  handleSelectChemical(null);
                }}
              />
            </div>
          </div>
        </div>
      </header>

      {/* Main Content */}
      <main className="flex-1 max-w-6xl mx-auto w-full p-4 md:p-6 grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        {/* Left Column: Search Results (Now a Grid) */}
        <div className="bg-white rounded-xl shadow-xl border border-gray-100 overflow-hidden flex flex-col h-[500px] lg:h-[calc(100vh-140px)]">
          <div className="p-4 border-b border-gray-100 bg-gray-50 flex justify-between items-center">
            <span className="font-bold text-blue-900 uppercase tracking-wider text-sm">Chemical List</span>
            <span className="text-xs font-bold px-3 py-1 bg-yellow-100 text-yellow-800 rounded-full border border-yellow-200">
              {filteredChemicals.length} Found
            </span>
          </div>
          <div className="overflow-y-auto flex-1 p-3 grid grid-cols-2 md:grid-cols-3 gap-2"> {/* CHANGED to grid layout */}
            {filteredChemicals.length === 0 ? (
              <div className="col-span-full text-center p-10 text-gray-400">
                <p>No chemicals found matching "{searchTerm}"</p>
              </div>
            ) : (
              filteredChemicals.map((chem) => (
                <button
                  key={chem.id}
                  onClick={() => handleSelectChemical(chem)}
                  className={`w-full text-center p-2 rounded-lg border transition-all duration-200 flex flex-col items-center justify-center group h-24
                    ${selectedChemical?.id === chem.id 
                      ? 'bg-blue-900 border-yellow-500 text-white shadow-md ring-1 ring-yellow-500' 
                      : 'bg-white border-gray-200 hover:bg-blue-50 hover:border-blue-300'
                    }`}
                >
                  <span className="text-sm font-semibold truncate w-full px-1">{chem.name}</span>
                  <div className={`mt-1 flex items-center gap-1 text-[10px] px-2 py-0.5 rounded font-bold
                    ${selectedChemical?.id === chem.id ? 'bg-yellow-500 text-blue-900' : 'bg-gray-100 text-gray-500 group-hover:bg-white'}
                  `}>
                    CUB. {chem.cubicle}
                  </div>
                </button>
              ))
            )}
          </div>
        </div>

        {/* Right Column: Visual Locator Map */}
        <div className="lg:col-span-2 flex flex-col gap-6">
          
          {/* Top Panel: Selection & AI Actions */}
          <div className="bg-white rounded-xl shadow-xl border border-gray-100 p-6">
            <div className="mb-4 border-b border-gray-100 pb-4 flex justify-between items-start">
               <div>
                  <h2 className="text-xl font-bold text-blue-900 flex items-center gap-2">
                    <MapPin className="text-yellow-500 fill-yellow-500" />
                    Locator & Assistant
                  </h2>
                  <p className="text-sm text-gray-500 mt-1">Select a chemical to view location and ask AI</p>
               </div>
            </div>

            {selectedChemical ? (
              <div className="flex flex-col gap-4">
                {/* Location Banner */}
                <div className="bg-gradient-to-r from-blue-50 to-white border-l-4 border-yellow-500 text-blue-900 p-4 rounded-r-lg shadow-sm flex flex-col sm:flex-row items-center justify-between gap-4">
                  <div className="flex items-center gap-4">
                    <div className="bg-blue-900 text-white p-3 rounded-full shadow-md border-2 border-yellow-400">
                      <Archive size={24} />
                    </div>
                    <div>
                      <span className="block font-bold text-xl leading-tight text-blue-900">{selectedChemical.name}</span>
                      <span className="text-sm text-blue-700">
                        Located in: <strong className="text-blue-900 bg-yellow-200 px-1 rounded">{selectedChemical.side} Side</strong>, Cubicle <strong className="text-blue-900 bg-yellow-200 px-1 rounded">{selectedChemical.cubicle}</strong>
                      </span>
                    </div>
                  </div>
                </div>

                {/* AI Action Buttons */}
                <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
                  <button 
                    onClick={handleGetSafetyInfo}
                    disabled={isAiLoading}
                    className="flex items-center justify-center gap-2 px-4 py-3 bg-blue-50 text-blue-900 border border-blue-200 rounded-lg hover:bg-blue-100 hover:border-blue-300 transition-all font-bold shadow-sm"
                  >
                    {isAiLoading && aiMode === 'safety' ? <Loader2 className="animate-spin" size={18} /> : <ShieldAlert size={18} className="text-blue-600" />}
                    Get Safety Snapshot
                  </button>
                  <button 
                    onClick={handleGetUses}
                    disabled={isAiLoading}
                    className="flex items-center justify-center gap-2 px-4 py-3 bg-yellow-50 text-yellow-900 border border-yellow-200 rounded-lg hover:bg-yellow-100 hover:border-yellow-300 transition-all font-bold shadow-sm"
                  >
                     {isAiLoading && aiMode === 'uses' ? <Loader2 className="animate-spin" size={18} /> : <FlaskConical size={18} className="text-yellow-600" />}
                    Suggest Experiments
                  </button>
                </div>

                {/* AI Response Area */}
                {(aiContent || isAiLoading) && (
                   <div className="mt-2 bg-gray-50 rounded-lg p-5 border border-gray-200 relative min-h-[100px] transition-all shadow-inner">
                      {isAiLoading ? (
                        <div className="flex flex-col items-center justify-center h-24 text-gray-400 gap-2">
                          <Loader2 className="animate-spin text-yellow-500" size={32} />
                          <span className="text-sm font-medium text-blue-900 animate-pulse">Consulting Gemini AI...</span>
                        </div>
                      ) : (
                        <div>
                          <h4 className="font-bold text-sm uppercase tracking-wider mb-3 flex items-center gap-2 border-b pb-2">
                            {aiMode === 'safety' ? (
                              <span className="text-blue-800 flex items-center gap-2"><ShieldAlert size={16} /> AI Safety Report</span>
                            ) : (
                              <span className="text-yellow-700 flex items-center gap-2"><Sparkles size={16} /> AI Experiment Ideas</span>
                            )}
                          </h4>
                          <div className="prose prose-sm max-w-none text-slate-700 whitespace-pre-wrap leading-relaxed">
                            {aiContent}
                          </div>
                        </div>
                      )}
                   </div>
                )}
              </div>
            ) : (
              <div className="text-center py-10 bg-gray-50 rounded-lg border-2 border-dashed border-gray-200 text-gray-400">
                <Search className="mx-auto h-10 w-10 mb-3 opacity-30 text-blue-900" />
                <p className="font-medium">Select a chemical from the list to see details</p>
              </div>
            )}
          </div>

          {/* Bottom Panel: Cabinet Map */}
          <div className="bg-white rounded-xl shadow-xl border border-gray-100 p-6 flex-1 flex flex-col min-h-[400px]">
             <h3 className="font-bold text-blue-900 mb-6 uppercase tracking-wider text-sm flex items-center gap-2">
                <Archive size={16} className="text-yellow-500" /> Visual Map
             </h3>
             <div className="flex-1 flex gap-4 sm:gap-8 justify-center items-start overflow-y-auto">
              {/* LEFT SIDE CABINET */}
              <div className="flex flex-col items-center w-1/2">
                <h3 className="mb-4 font-bold text-xs sm:text-sm text-gray-500 flex items-center gap-2">
                  <ArrowLeft size={14} /> LEFT SIDE
                </h3>
                <div className="w-full grid grid-cols-2 sm:grid-cols-3 gap-2 sm:gap-3 bg-gray-50 p-3 rounded-xl border border-gray-200">
                  {shelves.left.map((shelfNum) => {
                    const isTarget = selectedChemical?.side === 'LEFT' && selectedChemical?.cubicle === shelfNum;
                    return (
                      <div 
                        key={`L-${shelfNum}`}
                        className={`aspect-square rounded-lg flex flex-col items-center justify-center border-2 transition-all duration-300
                          ${isTarget 
                            ? 'bg-yellow-400 border-yellow-600 text-blue-900 shadow-xl scale-110 z-10' 
                            : 'bg-white border-gray-200 text-gray-300'
                          }
                        `}
                      >
                        <Archive size={isTarget ? 24 : 16} />
                        <span className={`text-xs sm:text-sm font-bold mt-1 ${isTarget ? 'text-blue-900' : 'text-gray-400'}`}>
                          {shelfNum}
                        </span>
                      </div>
                    );
                  })}
                </div>
              </div>

              {/* DIVIDER */}
              <div className="w-px bg-gray-200 h-full mx-1"></div>

              {/* RIGHT SIDE CABINET */}
              <div className="flex flex-col items-center w-1/2">
                <h3 className="mb-4 font-bold text-xs sm:text-sm text-gray-500 flex items-center gap-2">
                  RIGHT SIDE <ArrowRight size={14} />
                </h3>
                <div className="w-full grid grid-cols-2 sm:grid-cols-3 gap-2 sm:gap-3 bg-gray-50 p-3 rounded-xl border border-gray-200">
                  {shelves.right.map((shelfNum) => {
                    const isTarget = selectedChemical?.side === 'RIGHT' && selectedChemical?.cubicle === shelfNum;
                    return (
                      <div 
                        key={`R-${shelfNum}`}
                        className={`aspect-square rounded-lg flex flex-col items-center justify-center border-2 transition-all duration-300
                          ${isTarget 
                            ? 'bg-yellow-400 border-yellow-600 text-blue-900 shadow-xl scale-110 z-10' 
                            : 'bg-white border-gray-200 text-gray-300'
                          }
                        `}
                      >
                        <Archive size={isTarget ? 24 : 16} />
                        <span className={`text-xs sm:text-sm font-bold mt-1 ${isTarget ? 'text-blue-900' : 'text-gray-400'}`}>
                          {shelfNum}
                        </span>
                      </div>
                    );
                  })}
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  );
}
