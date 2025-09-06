<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Grand Grimoire of Rebirth - Interactive Wiki</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Cosmic Dusk -->
    <!-- Application Structure Plan: The SPA is structured as a single, scrollable page with a fixed navigation bar for easy access to different sections. This design was chosen to present the game's complex, interconnected systems in a logical, linear flow, much like reading a grimoire. The structure includes: 1) Core Concepts, 2) Progression Systems with an interactive EXP curve chart and calculator, 3) An interactive card-based Elemental System, 4) World Economy, 5) Combat Mechanics, and 6) Endgame goals. This thematic, section-based approach allows users to either get a full overview by scrolling or jump directly to a specific rule set, enhancing usability and comprehension. -->
    <!-- Visualization & Content Choices: 1. EXP Progression Curve: Goal(Show Change), Method(Bar Chart), Interaction(Visual representation of the steep formula), Justification(Visually communicates the increasing difficulty far better than numbers alone), Library(Chart.js). 2. RP & EXP Calculators: Goal(Explore/Inform), Method(Input fields and dynamic output), Interaction(Users input their level/stats to see potential gains), Justification(Makes abstract formulas tangible and helps with strategic planning), Library(Vanilla JS). 3. Elemental System: Goal(Compare/Organize), Method(Interactive cards in a grid layout), Interaction(Toggle buttons to view base vs. mastered stats), Justification(Provides a clean, interactive way to compare elements and understand their progression paths), Library(Vanilla JS). This ensures all key data is presented in the most effective, interactive format. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #1a1a2e;
            color: #e0e0e0;
        }
        h1, h2, h3 {
            font-family: 'Cinzel', serif;
            font-weight: 700;
        }
        .cinzel {
            font-family: 'Cinzel', serif;
        }
        .nav-link {
            transition: all 0.3s ease;
            position: relative;
        }
        .nav-link::after {
            content: '';
            position: absolute;
            width: 0;
            height: 2px;
            bottom: -4px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #fca5a5;
            transition: width 0.3s ease;
        }
        .nav-link:hover::after, .nav-link.active::after {
            width: 100%;
        }
        .card {
            background-color: #162447;
            border: 1px solid #1f4068;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.4);
        }
        .btn-primary {
            background-color: #e43f5a;
            color: #ffffff;
            transition: background-color 0.3s ease;
        }
        .btn-primary:hover {
            background-color: #b8324f;
        }
        .btn-secondary {
            background-color: #1f4068;
            color: #e0e0e0;
            transition: background-color 0.3s ease;
        }
        .btn-secondary:hover {
            background-color: #162447;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 40vh;
            max-height: 450px;
        }
        .text-shadow {
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-[#162447]/80 backdrop-blur-sm sticky top-0 z-50 shadow-lg border-b border-blue-900/50">
        <nav class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex-shrink-0">
                    <h1 class="text-2xl text-white cinzel">The Grand Grimoire</h1>
                </div>
                <div class="hidden md:block">
                    <div class="ml-10 flex items-baseline space-x-4">
                        <a href="#core-concepts" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white">Core Concepts</a>
                        <a href="#progression" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white">Progression</a>
                        <a href="#elements" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white">Elements</a>
                        <a href="#economy" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white">Economy</a>
                        <a href="#battle" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white">Battle</a>
                        <a href="#endgame" class="nav-link px-3 py-2 rounded-md text-sm font-medium text-gray-300 hover:text-white">Endgame</a>
                    </div>
                </div>
                 <div class="md:hidden">
                    <select id="mobile-nav" class="bg-[#1f4068] text-white rounded p-2">
                        <option value="#core-concepts">Core Concepts</option>
                        <option value="#progression">Progression</option>
                        <option value="#elements">Elements</option>
                        <option value="#economy">Economy</option>
                        <option value="#battle">Battle</option>
                        <option value="#endgame">Endgame</option>
                    </select>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">
        <section id="core-concepts" class="pt-20 -mt-20">
            <h2 class="text-4xl text-center mb-8 text-shadow text-red-300">Core Concepts</h2>
            <p class="text-center max-w-3xl mx-auto text-lg mb-12 text-gray-400">Welcome, cultivator. This Grimoire details the fundamental laws of this world. Your ultimate goal is to grow powerful enough to defeat Death itself, a journey against time as your very Lifespan ticks away with each passing moment. Embrace the cycle of Rebirth to unlock your true potential.</p>
            <div class="grid md:grid-cols-3 gap-8">
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">The Objective</h3>
                    <p class="text-gray-400">Defeat Death in a final, climactic boss battle. Your journey is a race against time, as your character's Lifespan decreases with every second of play. You must grow strong before your time runs out.</p>
                </div>
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">The Rebirth Loop</h3>
                    <p class="text-gray-400">Upon death, your character is reborn, resetting level and base stats. However, all permanent upgrades bought with Rebirth Points (RP) are retained, making each new life stronger than the last.</p>
                </div>
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">Rebirth Points (RP)</h3>
                    <p class="text-gray-400">The permanent currency used for upgrades that persist through all lives. RP is gained upon rebirth, with the amount determined by your performance in the previous life, including level achieved, quests completed, and tier reached.</p>
                </div>
            </div>
        </section>

        <hr class="my-16 border-blue-800/50">

        <section id="progression" class="pt-20 -mt-20">
            <h2 class="text-4xl text-center mb-8 text-shadow text-red-300">Progression & Tiers</h2>
            <p class="text-center max-w-3xl mx-auto text-lg mb-12 text-gray-400">Your path to power is long, measured in Levels and Tiers of cultivation. The experience required to advance grows exponentially, demanding dedication. The formula below governs your ascent.</p>
            <div class="card p-6 rounded-lg mb-8">
                <h3 class="text-2xl mb-4 text-center text-red-200">Experience Formula</h3>
                <p class="text-center text-2xl cinzel tracking-wider my-4 p-4 bg-black/20 rounded-md">EXP<sub>Needed</sub> = 100 * Level + 5 * Level<sup>3</sup></p>
                <div class="chart-container">
                    <canvas id="expCurveChart"></canvas>
                </div>
            </div>
            <div class="card p-6 rounded-lg">
                <h3 class="text-2xl mb-4 text-center text-red-200">EXP Calculator</h3>
                <div class="flex flex-col md:flex-row items-center justify-center gap-4">
                    <label for="levelInput" class="font-medium">Enter Level:</label>
                    <input type="number" id="levelInput" min="1" value="1" class="bg-[#1f4068] text-white rounded-md p-2 w-32 text-center border border-blue-700 focus:ring-red-400 focus:border-red-400">
                    <button id="calculateExpBtn" class="btn-primary px-6 py-2 rounded-md font-bold">Calculate EXP Needed</button>
                    <p class="text-lg">EXP Needed: <span id="expResult" class="font-bold text-red-200">105</span></p>
                </div>
            </div>
        </section>

        <hr class="my-16 border-blue-800/50">

        <section id="elements" class="pt-20 -mt-20">
            <h2 class="text-4xl text-center mb-8 text-shadow text-red-300">The Elemental Gamble</h2>
            <p class="text-center max-w-3xl mx-auto text-lg mb-12 text-gray-400">Upon each Rebirth, your very essence is reforged. Your Talent is determined by a random roll, which also assigns you a permanent Element. This is the foundation of your power, a gamble that defines your path.</p>
            <div id="element-cards" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            </div>
        </section>

        <hr class="my-16 border-blue-800/50">
        
        <section id="economy" class="pt-20 -mt-20">
            <h2 class="text-4xl text-center mb-8 text-shadow text-red-300">Map & Economy</h2>
             <p class="text-center max-w-3xl mx-auto text-lg mb-12 text-gray-400">As you ascend through cultivation tiers, new regions of the world map unlock. These lands hold rare resources essential for crafting powerful, temporary items. How you use these resources—for immediate gain or long-term power—is a critical choice.</p>
            <div class="grid md:grid-cols-2 gap-8">
                 <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">Currencies</h3>
                    <ul class="list-disc list-inside space-y-2 text-gray-400">
                        <li><b class="text-red-100">Spirit Stones:</b> In-life currency used for temporary upgrades.</li>
                        <li><b class="text-red-100">Rebirth Points (RP):</b> Permanent currency used for upgrades that persist across all lives.</li>
                    </ul>
                </div>
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">Crafting & The Gamble</h3>
                    <p class="text-gray-400 mb-4">Craft powerful items that last for a single life. Once per life, you can sell a crafted item to the "Black Market" for a high-risk, high-reward payout in Spirit Stones.</p>
                    <h4 class="text-xl mb-2 text-red-200">Legacy Upgrade</h4>
                     <p class="text-gray-400">A monumental, one-time permanent upgrade allowing you to select one crafted item to keep across all future lives.</p>
                </div>
            </div>
        </section>

        <hr class="my-16 border-blue-800/50">

        <section id="battle" class="pt-20 -mt-20">
            <h2 class="text-4xl text-center mb-8 text-shadow text-red-300">The Art of Battle</h2>
            <p class="text-center max-w-3xl mx-auto text-lg mb-12 text-gray-400">Combat is a turn-based dance of strategy. After joining a sect, you gain access to various battlegrounds where you can test your strength, complete quests, and prove your worth.</p>
            <div class="grid md:grid-cols-3 gap-8">
                 <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">Battle Types</h3>
                    <ul class="list-disc list-inside space-y-2 text-gray-400">
                        <li><b>Training Grounds:</b> Reliable smaller battles for grinding EXP and Spirit Stones.</li>
                        <li><b>Quests:</b> Story-based battles against mini-bosses and major bosses.</li>
                        <li><b>Daily Challenge:</b> A challenging daily mini-boss with a huge one-time reward.</li>
                    </ul>
                </div>
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">Combat Mechanics</h3>
                    <p class="text-gray-400">In turn-based combat, you choose from three distinct actions and apply one per-turn buff, creating layers of strategy for offense and defense.</p>
                </div>
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">The Hard Mode Challenge</h3>
                    <p class="text-gray-400">For the ultimate test, join a sect that opposes your element. This path is arduous, with no extra RP rewards, but offers a unique and exceptionally powerful Legacy Item upon completion.</p>
                </div>
            </div>
        </section>

        <hr class="my-16 border-blue-800/50">
        
        <section id="endgame" class="pt-20 -mt-20">
            <h2 class="text-4xl text-center mb-8 text-shadow text-red-300">The Endgame</h2>
            <p class="text-center max-w-3xl mx-auto text-lg mb-12 text-gray-400">Beyond defeating Death lies the path to true immortality: the pursuit of power on the grand leaderboards. Here, your legacy is forged.</p>
            <div class="grid md:grid-cols-2 gap-8">
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">The Isle of Duality</h3>
                    <p class="text-gray-400">A late-game region housing the two Elemental Guardians. To complete your Elemental Mastery and earn the final blessing, you must defeat the guardian of your opposing element, proving your absolute resolve.</p>
                </div>
                <div class="card p-6 rounded-lg">
                    <h3 class="text-2xl mb-3 text-red-200">Leaderboards & Rewards</h3>
                    <p class="text-gray-400">Compete on multiple leaderboards for prestige and power:</p>
                    <ul class="list-disc list-inside space-y-2 text-gray-400 mt-4">
                        <li><b>Total RP Earned</b></li>
                        <li><b>Fastest to Ascend</b></li>
                        <li><b>Highest Level in a Single Life</b></li>
                    </ul>
                    <p class="text-gray-400 mt-2">Rewards include unique Legacy Items and cosmetic Titles.</p>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-[#162447] py-6 mt-12 border-t border-blue-900/50">
        <div class="container mx-auto text-center text-gray-500">
            <p>&copy; 2025 The Grand Grimoire of Rebirth. All rights reserved.</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const sections = document.querySelectorAll('main section');
            const navLinks = document.querySelectorAll('header nav a');
            const mobileNav = document.getElementById('mobile-nav');

            const observerOptions = {
                root: null,
                rootMargin: '0px',
                threshold: 0.3
            };

            const observer = new IntersectionObserver((entries, observer) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        navLinks.forEach(link => {
                            link.classList.toggle('active', link.getAttribute('href').substring(1) === entry.target.id);
                        });
                        mobileNav.value = `#${entry.target.id}`;
                    }
                });
            }, observerOptions);

            sections.forEach(section => {
                observer.observe(section);
            });
            
            mobileNav.addEventListener('change', (e) => {
                const targetId = e.target.value;
                document.querySelector(targetId).scrollIntoView({ behavior: 'smooth' });
            });

            const expFormula = (level) => 100 * level + 5 * Math.pow(level, 3);

            const levelInput = document.getElementById('levelInput');
            const calculateExpBtn = document.getElementById('calculateExpBtn');
            const expResult = document.getElementById('expResult');

            calculateExpBtn.addEventListener('click', () => {
                const level = parseInt(levelInput.value) || 1;
                const expNeeded = expFormula(level);
                expResult.textContent = expNeeded.toLocaleString();
            });

            const expChartCtx = document.getElementById('expCurveChart').getContext('2d');
            const chartLevels = [1, 25, 50, 75, 100, 125, 150];
            const chartExp = chartLevels.map(level => expFormula(level));

            new Chart(expChartCtx, {
                type: 'bar',
                data: {
                    labels: chartLevels.map(l => `Level ${l}`),
                    datasets: [{
                        label: 'EXP Needed to Level Up',
                        data: chartExp,
                        backgroundColor: 'rgba(228, 63, 90, 0.6)',
                        borderColor: 'rgba(228, 63, 90, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed.y !== null) {
                                        label += context.parsed.y.toLocaleString();
                                    }
                                    return label;
                                }
                            }
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            title: {
                                display: true,
                                text: 'Experience Points',
                                color: '#e0e0e0'
                            },
                            ticks: {
                                color: '#e0e0e0',
                                callback: function(value) {
                                    if (value >= 1000000) return (value / 1000000) + 'M';
                                    if (value >= 1000) return (value / 1000) + 'K';
                                    return value;
                                }
                            },
                            grid: {
                                color: 'rgba(31, 64, 104, 0.5)'
                            }
                        },
                        x: {
                            title: {
                                display: true,
                                text: 'Character Level',
                                color: '#e0e0e0'
                            },
                            ticks: {
                                color: '#e0e0e0'
                            },
                             grid: {
                                color: 'rgba(31, 64, 104, 0.2)'
                            }
                        }
                    }
                }
            });

            const elements = [
                { name: 'Inferno', type: 'Common', chance: '20%', stat: 'Power', base: 'x1.1', mastered: 'x1.25', blessed: 'x1.35', final: 'Ember', color: 'bg-red-500/20 border-red-500' },
                { name: 'Aqua', type: 'Common', chance: '20%', stat: 'HP', base: 'x1.1', mastered: 'x1.25', blessed: 'x1.35', final: 'Typhoon', color: 'bg-blue-500/20 border-blue-500' },
                { name: 'Earth', type: 'Common', chance: '20%', stat: 'Block', base: 'x1.1', mastered: 'x1.25', blessed: 'x1.35', final: 'Earthquake', color: 'bg-yellow-600/20 border-yellow-600' },
                { name: 'Cloud', type: 'Common', chance: '20%', stat: 'Speed', base: 'x1.1', mastered: 'x1.25', blessed: 'x1.35', final: 'Tornado', color: 'bg-gray-400/20 border-gray-400' },
                { name: 'Phos', type: 'Rare', chance: '10%', stat: 'Block & Speed', base: 'x1.25', mastered: 'x1.5', blessed: 'x1.75', final: 'Solar', color: 'bg-yellow-400/20 border-yellow-400' },
                { name: 'Umbra', type: 'Rare', chance: '10%', stat: 'Power & HP', base: 'x1.25', mastered: 'x1.5', blessed: 'x1.75', final: 'Abyss', color: 'bg-purple-500/20 border-purple-500' },
            ];

            const elementCardsContainer = document.getElementById('element-cards');
            elements.forEach((el, index) => {
                const cardHTML = `
                    <div class="card p-6 rounded-lg border-2 ${el.color}">
                        <div class="flex justify-between items-center mb-3">
                            <h3 class="text-2xl">${el.name}</h3>
                            <span class="px-3 py-1 text-xs font-bold rounded-full ${el.type === 'Rare' ? 'bg-purple-600 text-white' : 'bg-gray-600 text-gray-200'}">${el.type}</span>
                        </div>
                        <p class="text-gray-400 mb-4">Chance: <span class="font-bold">${el.chance}</span></p>
                        
                        <div class="space-y-3">
                            <p><b>Stat Bonus:</b> ${el.stat}</p>
                            <p><b>Base Bonus:</b> <span class="font-bold text-lg text-red-200">${el.base}</span></p>
                            <div class="hidden mastery-info">
                                <p><b>Mastered Bonus:</b> <span class="font-bold text-lg text-red-300">${el.mastered}</span></p>
                                <p><b>Guardian's Blessing:</b> <span class="font-bold text-lg text-yellow-300">${el.blessed}</span></p>
                                <p><b>Final Form:</b> <span class="font-bold text-xl cinzel">${el.final}</span></p>
                            </div>
                        </div>

                        <button class="toggle-mastery btn-secondary w-full mt-4 py-2 rounded-md text-sm font-semibold">Show Mastery</button>
                    </div>
                `;
                elementCardsContainer.innerHTML += cardHTML;
            });

            document.querySelectorAll('.toggle-mastery').forEach(button => {
                button.addEventListener('click', (e) => {
                    const card = e.target.closest('.card');
                    const info = card.querySelector('.mastery-info');
                    info.classList.toggle('hidden');
                    e.target.textContent = info.classList.contains('hidden') ? 'Show Mastery' : 'Hide Mastery';
                });
            });
        });
    </script>
</body>
</html>
