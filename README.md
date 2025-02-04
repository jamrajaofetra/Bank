# Bank
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Simulateur de Prêt Bancaire</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f0f0f0;
        }

        .calculator {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        .input-group {
            margin-bottom: 15px;
        }

        label {
            display: inline-block;
            width: 150px;
        }

        input {
            padding: 5px;
            width: 200px;
        }

        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .results {
            margin-top: 20px;
            display: none;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: center;
        }

        th {
            background-color: #4CAF50;
            color: white;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <h2>Simulateur de prêt immobilier</h2>
        
        <div class="input-group">
            <label>Montant du prêt (€):</label>
            <input type="number" id="loanAmount" min="0">
        </div>
        
        <div class="input-group">
            <label>Taux d'intérêt annuel (%):</label>
            <input type="number" id="interestRate" min="0" step="0.1">
        </div>
        
        <div class="input-group">
            <label>Durée (années):</label>
            <input type="number" id="loanTerm" min="1">
        </div>

        <button onclick="calculateLoan()">Calculer</button>
        <button onclick="resetForm()">Réinitialiser</button>

        <div class="results" id="results">
            <h3>Résultats:</h3>
            <p>Mensualité: <span id="monthlyPayment"></span> €</p>
            <p>Total des intérêts: <span id="totalInterest"></span> €</p>
            <p>Montant total remboursé: <span id="totalAmount"></span> €</p>
            
            <h3>Tableau d'amortissement</h3>
            <table id="amortizationTable">
                <tr>
                    <th>Mois</th>
                    <th>Capital remboursé</th>
                    <th>Intérêts</th>
                    <th>Capital restant</th>
                </tr>
            </table>
        </div>
    </div>

    <script>
        function calculateLoan() {
            // Récupération des valeurs
            const loanAmount = parseFloat(document.getElementById('loanAmount').value);
            const annualInterestRate = parseFloat(document.getElementById('interestRate').value);
            const loanTerm = parseFloat(document.getElementById('loanTerm').value);

            // Validation des entrées
            if (!loanAmount || !annualInterestRate || !loanTerm) {
                alert("Veuillez remplir tous les champs!");
                return;
            }

            // Calculs
            const monthlyInterestRate = annualInterestRate / 100 / 12;
            const numberOfPayments = loanTerm * 12;
            
            // Calcul de la mensualité
            const monthlyPayment = 
                (loanAmount * monthlyInterestRate) / 
                (1 - Math.pow(1 + monthlyInterestRate, -numberOfPayments));
            
            // Affichage des résultats principaux
            document.getElementById('monthlyPayment').textContent = monthlyPayment.toFixed(2);
            document.getElementById('totalInterest').textContent = 
                (monthlyPayment * numberOfPayments - loanAmount).toFixed(2);
            document.getElementById('totalAmount').textContent = 
                (monthlyPayment * numberOfPayments).toFixed(2);
            
            // Génération du tableau d'amortissement
            generateAmortizationTable(loanAmount, monthlyInterestRate, monthlyPayment, numberOfPayments);
            
            // Affichage des résultats
            document.getElementById('results').style.display = 'block';
        }

        function generateAmortizationTable(loanAmount, monthlyRate, monthlyPayment, numberOfPayments) {
            const table = document.getElementById('amortizationTable');
            table.innerHTML = `<tr>
                <th>Mois</th>
                <th>Capital remboursé</th>
                <th>Intérêts</th>
                <th>Capital restant</th>
            </tr>`;

            let remainingBalance = loanAmount;
            
            for (let month = 1; month <= numberOfPayments; month++) {
                const interest = remainingBalance * monthlyRate;
                const principal = monthlyPayment - interest;
                remainingBalance -= principal;

                const row = table.insertRow();
                row.insertCell().textContent = month;
                row.insertCell().textContent = principal.toFixed(2);
                row.insertCell().textContent = interest.toFixed(2);
                row.insertCell().textContent = remainingBalance.toFixed(2);
            }
        }

        function resetForm() {
            document.getElementById('loanAmount').value = '';
            document.getElementById('interestRate').value = '';
            document.getElementById('loanTerm').value = '';
            document.getElementById('results').style.display = 'none';
            document.getElementById('amortizationTable').innerHTML = `<tr>
                <th>Mois</th>
                <th>Capital remboursé</th>
                <th>Intérêts</th>
                <th>Capital restant</th>
            </tr>`;
        }
    </script>
</body>
</html>
```
