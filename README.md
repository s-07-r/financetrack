

<!DOCTYPE html>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Finance Tracker</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Finance Tracker</h1>
        <form id="transaction-form">
            <input type="date" id="date" required>
            <input type="text" id="category" placeholder="Category" required>
            <input type="number" id="amount" placeholder="Amount" required>
            <button type="submit">Add Transaction</button>
        </form>
        
        <h2>Transaction History</h2>
        <table>
            <thead>
                <tr>
                    <th>Date</th>
                    <th>Category</th>
                    <th>Amount</th>
                </tr>
            </thead>
            <tbody id="transaction-list">
                <!-- Transactions will be inserted here -->
            </tbody>
        </table>
        
        <h3>Current Balance: $<span id="balance">0.00</span></h3>
    </div>

    <script src="script.js"></script>
</body>
</html>
<style>
    body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    text-align: center;
    padding: 20px;
}

.container {
    width: 50%;
    margin: auto;
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

input, button {
    margin: 5px;
    padding: 10px;
    width: calc(33% - 22px);
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

th, td {
    padding: 10px;
    border: 1px solid #ddd;
}

th {
    background-color: #007bff;
    color: white;
}

</style>
<script>
    document.getElementById("transaction-form").addEventListener("submit", function(event) {
    event.preventDefault();

    let date = document.getElementById("date").value;
    let category = document.getElementById("category").value;
    let amount = parseFloat(document.getElementById("amount").value);

    if (!date || !category || isNaN(amount)) {
        alert("Please fill in all fields.");
        return;
    }

    let transaction = { date, category, amount };

    fetch("http://localhost:5000/addTransaction", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(transaction)
    })
    .then(response => response.json())
    .then(data => {
        updateTransactionList(data.transactions);
        document.getElementById("balance").innerText = data.balance.toFixed(2);
    })
    .catch(error => console.error("Error:", error));

    document.getElementById("transaction-form").reset();
});

function updateTransactionList(transactions) {
    let transactionList = document.getElementById("transaction-list");
    transactionList.innerHTML = "";

    transactions.forEach(t => {
        let row = `<tr><td>${t.date}</td><td>${t.category}</td><td>${t.amount}</td></tr>`;
        transactionList.innerHTML += row;
    });
}

</script>
