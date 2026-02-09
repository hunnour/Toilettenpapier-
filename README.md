<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Toilettenpapier Preis-Check</title>
    <style>
        body { font-family: 'Segoe UI', sans-serif; background-color: #f0f4f8; display: flex; justify-content: center; padding: 20px; }
        .container { width: 100%; max-width: 900px; background: white; padding: 25px; border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        h2 { color: #2c3e50; text-align: center; border-bottom: 2px solid #3498db; padding-bottom: 10px; }
        
        .compare-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 15px; margin-top: 20px; }
        .product-card { background: #f9f9f9; border: 1px solid #ddd; padding: 15px; border-radius: 8px; transition: 0.3s; }
        .product-card.winner { border: 2px solid #27ae60; background: #e8f5e9; transform: scale(1.02); }
        
        .product-card h3 { margin-top: 0; font-size: 1rem; color: #34495e; }
        label { display: block; font-size: 0.8rem; margin-top: 10px; color: #666; }
        input { width: 100%; padding: 8px; margin-top: 4px; border: 1px solid #ccc; border-radius: 4px; box-sizing: border-box; font-weight: bold; }
        
        .result { margin-top: 15px; padding: 10px; background: #fff; border-radius: 4px; text-align: center; font-weight: bold; min-height: 40px; }
        .winner-label { color: #27ae60; display: none; font-size: 0.9rem; margin-bottom: 5px; }
        
        button { width: 100%; padding: 15px; background: #3498db; color: white; border: none; border-radius: 8px; font-size: 1.1rem; cursor: pointer; margin-top: 25px; font-weight: bold; }
        button:hover { background: #2980b9; }
        .footer-info { text-align: center; font-size: 0.8rem; color: #95a5a6; margin-top: 20px; }
    </style>
</head>
<body>

<div class="container">
    <h2>🧻 Toilettenpapier Preis-Vergleich</h2>
    
    <div class="compare-grid" id="productGrid">
        <script>
            for(let i=1; i<=5; i++) {
                document.write(`
                    <div class="product-card" id="card-${i}">
                        <div class="winner-label" id="win-text-${i}">🏆 GÜNSTIGSTER</div>
                        <h3>Typ ${i}</h3>
                        <label>Name / Marke</label>
                        <input type="text" id="name-${i}" placeholder="z.B. Soft&Gut">
                        
                        <label>Rollen Anzahl</label>
                        <input type="number" id="rolls-${i}" placeholder="0" oninput="calculate()">
                        
                        <label>Blatt pro Rolle</label>
                        <input type="number" id="sheets-${i}" placeholder="0" oninput="calculate()">
                        
                        <label>Preis (€)</label>
                        <input type="number" step="0.01" id="price-${i}" placeholder="0.00" oninput="calculate()">
                        
                        <div class="result" id="res-${i}">-</div>
                    </div>
                `);
            }
        </script>
    </div>

    <button onclick="calculate()">Jetzt vergleichen</button>

    <div class="footer-info">
        Berechnet den Preis pro 100 Blatt. Je niedriger der Wert, desto mehr sparst du!
    </div>
</div>

<script>
    function calculate() {
        let bestPrice = Infinity;
        let winnerIndex = -1;
        let results = [];

        for (let i = 1; i <= 5; i++) {
            const rolls = parseFloat(document.getElementById(`rolls-${i}`).value);
            const sheets = parseFloat(document.getElementById(`sheets-${i}`).value);
            const price = parseFloat(document.getElementById(`price-${i}`).value);
            const resDiv = document.getElementById(`res-${i}`);
            const card = document.getElementById(`card-${i}`);
            const winText = document.getElementById(`win-text-${i}`);

            // Reset Styles
            card.classList.remove('winner');
            winText.style.display = 'none';

            if (rolls > 0 && sheets > 0 && price > 0) {
                const totalSheets = rolls * sheets;
                const pricePer100 = (price / totalSheets) * 100;
                results[i] = pricePer100;
                resDiv.innerText = pricePer100.toFixed(3) + " € / 100 Blatt";

                if (pricePer100 < bestPrice) {
                    bestPrice = pricePer100;
                    winnerIndex = i;
                }
            } else {
                resDiv.innerText = "-";
                results[i] = null;
            }
        }

        // Gewinner markieren
        if (winnerIndex !== -1) {
            document.getElementById(`card-${winnerIndex}`).classList.add('winner');
            document.getElementById(`win-text-${winnerIndex}`).style.display = 'block';
        }
    }
</script>

</body>
</html>
