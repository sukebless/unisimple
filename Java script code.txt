Javascript
const CPI = 0.055; // Example CPI rate (5.5% as a decimal)

document.addEventListener('DOMContentLoaded', function() {
    document.querySelector('#calculateButton').addEventListener('click', validateAndCalculate);
});

function validateAndCalculate() {
    const salary = document.getElementById('salary').value.trim();
    const contributions = document.getElementById('contributions').value.trim();
    const tax = document.getElementById('tax').value.trim();
    const yearsToRetirement = document.getElementById('yearsToRetirement').value.trim();

    // Check if any required fields are empty
    if (!salary || !contributions || !tax || !yearsToRetirement) {
        alert("Please fill in all fields before calculating.");
        return;
    }

    const salaryNum = parseFloat(salary);
    const contributionsNum = parseFloat(contributions);
    const taxNum = parseFloat(tax);
    const yearsToRetirementNum = parseInt(yearsToRetirement);

    // Check for negative numbers and limit the amount to a max of 20 million
    if (salaryNum <= 0 || contributionsNum < 0 || taxNum < 0 || yearsToRetirementNum <= 0) {
        alert("Please enter positive numbers only.");
        return;
    }

    if (salaryNum > 20000000 || contributionsNum > 20000000 || taxNum > 20000000) {
        alert("Amounts cannot exceed 20 million.");
        return;
    }

    if (contributionsNum > salaryNum) {
        alert("Annual contributions cannot exceed the annual salary.");
        return;
    }

    if (taxNum > salaryNum) {
        alert("Annual tax cannot exceed the annual salary.");
        return;
    }

    calculateContributions();
}

function calculateContributions() {
    const salary = parseFloat(document.getElementById('salary').value);
    const contributions = parseFloat(document.getElementById('contributions').value);
    const tax = parseFloat(document.getElementById('tax').value);
    const percentage = parseFloat(document.getElementById('percentage').value) / 100;
    const yearsToRetirement = parseInt(document.getElementById('yearsToRetirement').value);

    const maxContribution = Math.min(0.275 * salary, 350000);
    const availableContributions = Math.max(tax - maxContribution, tax - contributions);
    const selectedContribution = availableContributions * percentage;

    animateCount('result', availableContributions, 'Available Contributions: R');
    const estimatedContribution = (2 / 3) * selectedContribution;
    const interest = estimatedContribution * CPI;
    const totalEstimatedFund = estimatedContribution + interest;
    const forecastedWithdrawal = selectedContribution / 3;

    animateCount('forecastResult', forecastedWithdrawal, 'Forecasted Withdrawal: R');
    const taxRate = getTaxRate(salary);
    const netAvailableWithdrawal = forecastedWithdrawal - (forecastedWithdrawal * taxRate);

    animateCount('netWithdrawalResult', netAvailableWithdrawal, 'Net Available Withdrawal: R');
    animateCount('estimatedFundResult', totalEstimatedFund, 'Estimated Retirement Fund Contribution: R');

    const retirementSavingsForecast = calculateRetirementSavingsForecast(estimatedContribution, yearsToRetirement);
    animateCount('retirementSavingsResult', retirementSavingsForecast, 'Retirement Savings Forecast: R');
}

function calculateRetirementSavingsForecast(contribution, years) {
    const totalSavings = contribution * (1 + CPI * years);
    return totalSavings;
}

function getTaxRate(salary) {
    if (salary <= 237100) return 0.18;
    if (salary <= 370500) return 0.26;
    if (salary <= 512800) return 0.31;
    if (salary <= 673000) return 0.36;
    if (salary <= 857900) return 0.39;
    if (salary <= 1817000) return 0.41;
    return 0.45;
}

function animateCount(elementId, value, prefix) {
    const element = document.getElementById(elementId);
    element.innerText = `${prefix}${value.toFixed(2)}`;
}
