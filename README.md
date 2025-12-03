<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chemical Inventory Locator</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for the result card for better visual separation */
        .chemical-card {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            transition: transform 0.3s ease;
        }
        .chemical-card:hover {
            transform: translateY(-4px);
        }
    </style>
    <script>
        // Set up Tailwind configuration for better default styling (using Inter font)
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                }
            }
        }
    </script>
</head>
<body class="bg-gray-50 min-h-screen font-sans">

    <div class="max-w-4xl mx-auto p-4 sm:p-6 lg:p-8">
        <header class="text-center py-6 bg-white rounded-lg shadow-md mb-8">
            <h1 class="text-3xl sm:text-4xl font-extrabold text-blue-800">
                Chemical Inventory Locator
            </h1>
            <p class="mt-2 text-gray-500 text-lg">Quickly find the storage location for any chemical.</p>
        </header>

        <!-- Search Input and Button -->
        <div class="bg-white p-6 rounded-lg shadow-xl mb-8">
            <div class="flex flex-col sm:flex-row gap-4">
                <input
                    type="text"
                    id="searchInput"
                    placeholder="Enter chemical name (e.g., IODINE, COPPER SULFATE)"
                    class="flex-grow p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 text-lg"
                >
                <button
                    id="searchButton"
                    onclick="filterChemicals()"
                    class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-6 rounded-lg transition duration-150 ease-in-out shadow-lg transform hover:scale-[1.02] active:scale-[0.98]"
                >
                    <svg class="w-5 h-5 inline mr-2 -mt-0.5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                    Search Inventory
                </button>
            </div>
        </div>

        <!-- Results Display Area -->
        <div id="resultsContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Results will be injected here by JavaScript -->
            <p id="initialMessage" class="md:col-span-2 lg:col-span-3 text-center text-gray-500 text-lg py-12">
                Start searching by typing a chemical name above.
            </p>
        </div>
    </div>

    <script type="text/javascript">
        // Hardcoded Data from CHEMICALS EX NAME.xlsx - Sheet1.csv
        // Note: Cabinet and Cubicle information is inferred from the CSV snippet's structure.
        const chemicals = [
            // CABINET -1 / RIGHT SIDE
            { name: "BROMOCRESOL GREEN INDICATOR", cabinet: "CABINET -1", side: "RIGHT", cubicle: "1" },
            { name: "BROMOTHYMOL BLUE", cabinet: "CABINET -1", side: "RIGHT", cubicle: "1" },
            { name: "POTASSIUM PERMANGANATE", cabinet: "CABINET -1", side: "RIGHT", cubicle: "1" },
            { name: "SODUIM BISULFATE", cabinet: "CABINET -1", side: "RIGHT", cubicle: "1" },
            { name: "SUDIUM ACETATE", cabinet: "CABINET -1", side: "RIGHT", cubicle: "2" },
            { name: "CRYSTAL VIOLET", cabinet: "CABINET -1", side: "RIGHT", cubicle: "3" },
            { name: "IODINE", cabinet: "CABINET -1", side: "RIGHT", cubicle: "3" },
            { name: "METHANE BLUE", cabinet: "CABINET -1", side: "RIGHT", cubicle: "3" },
            { name: "ISOPROPLY ALCOHOL", cabinet: "CABINET -1", side: "RIGHT", cubicle: "4" },
            { name: "GLUCOSE SOLUTION", cabinet: "CABINET -1", side: "RIGHT", cubicle: "5" },
            { name: "BENEDICT'S SOLUTION", cabinet: "CABINET -1", side: "RIGHT", cubicle: "6" },

            // CABINET -1 / LEFT SIDE (Inferred continuation for later rows like 188+)
            { name: "BARIUM SULFATE", cabinet: "CABINET -1", side: "LEFT", cubicle: "15" },
            { name: "CALCIUM CARBIDE", cabinet: "CABINET -1", side: "LEFT", cubicle: "15" },
            { name: "COPPER SULFATE", cabinet: "CABINET -1", side: "LEFT", cubicle: "15" },
            { name: "MANGANESE SULFATE", cabinet: "CABINET -1", side: "LEFT", cubicle: "15" },
            { name: "SODIUM METABISULFATE", cabinet: "CABINET -1", side: "LEFT", cubicle: "15" },
            { name: "BUFFERED FORMALIN", cabinet: "CABINET -1", side: "LEFT", cubicle: "16" },
            { name: "ETHYL ALCOHOL", cabinet: "CABINET -1", side: "LEFT", cubicle: "16" },
            { name: "AMMONIUM BROMIDE", cabinet: "CABINET -1", side: "LEFT", cubicle: "17" },
            { name: "AMMONIUM IODIDE", cabinet: "CABINET -1", side: "LEFT", cubicle: "17" },
        ];

        // Get elements
        const searchInput = document.getElementById('searchInput');
        const resultsContainer = document.getElementById('resultsContainer');
        const initialMessage = document.getElementById('initialMessage');

        // Function to render the results
        function renderResults(filteredList) {
            // Clear previous results and messages
            resultsContainer.innerHTML = '';
            
            if (filteredList.length === 0) {
                resultsContainer.innerHTML = `
                    <p class="md:col-span-2 lg:col-span-3 text-center text-red-500 text-lg py-12">
                        No chemicals found matching your search. Try a different name.
                    </p>
                `;
                return;
            }

            filteredList.forEach(chemical => {
                // Create the card element for each result
                const card = document.createElement('div');
                card.className = 'chemical-card bg-white p-6 rounded-xl border-l-4 border-blue-500';
                
                // Card content structure
                card.innerHTML = `
                    <h3 class="text-xl font-bold mb-4 text-blue-700 uppercase">${chemical.name}</h3>
                    
                    <div class="space-y-3 text-gray-700">
                        <!-- Cabinet Location -->
                        <div class="flex items-center">
                            <svg class="w-5 h-5 text-gray-400 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"></path></svg>
                            <span class="font-semibold">Cabinet:</span>
                            <span class="ml-2">${chemical.cabinet}</span>
                        </div>

                        <!-- Side Location -->
                        <div class="flex items-center">
                            <svg class="w-5 h-5 text-gray-400 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"></path></svg>
                            <span class="font-semibold">Side:</span>
                            <span class="ml-2 text-green-600 font-bold">${chemical.side}</span>
                        </div>

                        <!-- Cubicle Number -->
                        <div class="flex items-center">
                            <svg class="w-5 h-5 text-gray-400 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7h16M4 7l-2 2m2-2l2-2m2 2l-2-2m-2 2h4"></path></svg>
                            <span class="font-semibold">Cubicle:</span>
                            <span class="ml-2 text-lg bg-blue-100 px-2 py-0.5 rounded-full font-extrabold text-blue-600">${chemical.cubicle}</span>
                        </div>
                    </div>
                `;
                resultsContainer.appendChild(card);
            });
        }

        // Main search and filter function
        function filterChemicals() {
            const query = searchInput.value.trim().toLowerCase();
            
            if (query.length < 2) {
                // Show a hint if the query is too short
                resultsContainer.innerHTML = `
                    <p class="md:col-span-2 lg:col-span-3 text-center text-orange-500 text-lg py-12">
                        Please enter at least 2 characters to search.
                    </p>
                `;
                return;
            }

            const filtered = chemicals.filter(chemical => 
                chemical.name.toLowerCase().includes(query)
            );

            renderResults(filtered);
        }

        // Add event listener for Enter key on the input field
        searchInput.addEventListener('keypress', function (e) {
            if (e.key === 'Enter') {
                filterChemicals();
            }
        });

        // Initial render: show all chemicals by default or only the initial message
        window.onload = function() {
            // Uncomment the line below if you want to display all chemicals when the page loads:
            // renderResults(chemicals);
        };
    </script>
</body>
</html>
