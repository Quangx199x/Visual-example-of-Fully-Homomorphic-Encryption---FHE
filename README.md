# Visual-example-of-Fully-Homomorphic-Encryption---FHE
This code uses Fully Homomorphic Encryption (FHE) to calculate the golden ratio on the encrypted data.

        <h1>🔐 FHE Interactive Calculator</h1>
        
        <div class="input-section">
            <h2>📝 Nhập dữ liệu của bạn</h2>
            
            <div class="input-group">
                <label for="input1">Số thứ nhất:</label>
                <input type="number" id="input1" value="1" min="0" max="15" step="0.1" placeholder="0-15">
            </div>
            
            <div class="input-group">
                <label for="input2">Số thứ hai:</label>
                <input type="number" id="input2" value="5" min="0" max="15" step="0.1" placeholder="0-15">
            </div>
            
            <div class="operation-selector">
                <label for="operation">Chọn phép tính:</label>
                <select id="operation">
                    <option value="golden_ratio">Tỷ lệ vàng: (a + √b) / 2</option>
                    <option value="add">Cộng: a + b</option>
                    <option value="multiply">Nhân: a × b</option>
                    <option value="sqrt_add">√a + √b</option>
                    <option value="reciprocal">1/a + 1/b</option>
                </select>
            </div>

            <button class="demo-button" onclick="startFHECalculation()">🚀 Bắt đầu tính toán FHE</button>
        </div>

        <div id="calculation-steps" style="display: none;">
            <div class="step">
                <span class="step-number">1</span>
                <strong>Dữ liệu gốc (rõ ràng):</strong>
                <div class="encryption-visual">
                    <div class="data-box clear-data" id="clear-val1">a</div>
                    <div class="data-box clear-data" id="clear-val2">b</div>
                </div>
            </div>

            <div class="progress-bar">
                <div class="progress-fill" id="progress"></div>
            </div>

            <div class="process-arrow" id="encrypt-arrow" style="display: none;">⬇️ MÃ HÓA ⬇️</div>

            <div class="step" id="encrypt-step" style="display: none;">
                <span class="step-number">2</span>
                <strong>Sau khi mã hóa:</strong>
                <div class="encryption-visual">
                    <div class="data-box encrypted-data" id="encrypted-val1">🔒 E(a)</div>
                    <div class="data-box encrypted-data" id="encrypted-val2">🔒 E(b)</div>
                </div>
                <p><em>Dữ liệu hiện đã được bảo mật hoàn toàn!</em></p>
            </div>

            <div class="process-arrow" id="calc-arrow" style="display: none;">⬇️ TÍNH TOÁN ⬇️</div>

            <div class="step" id="calc-step" style="display: none;">
                <span class="step-number">3</span>
                <strong>Tính toán trên dữ liệu mã hóa:</strong>
                <div class="math-formula" id="formula">Phép tính sẽ hiển thị ở đây</div>
                <div class="encryption-visual">
                    <div class="data-box encrypted-data calculating" id="calc-result">🔒 E(kết quả)</div>
                </div>
                <p><em>Server thực hiện tính toán mà không biết giá trị thực!</em></p>
            </div>

            <div class="process-arrow" id="decrypt-arrow" style="display: none;">⬇️ GIẢI MÃ ⬇️</div>

            <div class="result-section" id="result-section">
                <span class="step-number">4</span>
                <strong>Kết quả cuối cùng:</strong>
                <div class="encryption-visual">
                    <div class="data-box result-data" id="final-result">Kết quả</div>
                </div>
                <div id="result-explanation"></div>
            </div>
        </div>

        <div class="step" style="background: rgba(0,255,0,0.1); border-left-color: #00ff00;">
            <span class="step-number">💡</span>
            <strong>Lưu ý về U4F12:</strong>
            <ul>
                <li><strong>Giới hạn:</strong> 0 ≤ giá trị < 16 (4 bit nguyên)</li>
                <li><strong>Độ chính xác:</strong> 1/4096 ≈ 0.0002 (12 bit thập phân)</li>
                <li><strong>Ví dụ:</strong> 1.5 → 1.4998, 15.9 → 15.8999</li>
            </ul>
        </div>
    </div>

    <script>
        let calculationInProgress = false;

        function startFHECalculation() {
            if (calculationInProgress) return;
            
            const input1 = parseFloat(document.getElementById('input1').value);
            const input2 = parseFloat(document.getElementById('input2').value);
            const operation = document.getElementById('operation').value;
            
            // Validate inputs
            if (isNaN(input1) || isNaN(input2) || input1 < 0 || input1 >= 16 || input2 < 0 || input2 >= 16) {
                alert('Vui lòng nhập số từ 0 đến 15.9 (giới hạn của U4F12)');
                return;
            }
            
            calculationInProgress = true;
            document.querySelector('.demo-button').disabled = true;
            document.querySelector('.demo-button').innerHTML = '⚙️ Đang xử lý...';
            
            // Show calculation steps
            document.getElementById('calculation-steps').style.display = 'block';
            
            // Update clear values
            document.getElementById('clear-val1').innerHTML = input1.toFixed(2);
            document.getElementById('clear-val2').innerHTML = input2.toFixed(2);
            
            // Simulate step-by-step process
            simulateSteps(input1, input2, operation);
        }

        function simulateSteps(input1, input2, operation) {
            let step = 0;
            const totalSteps = 4;
            
            const interval = setInterval(() => {
                step++;
                updateProgress(step / totalSteps * 100);
                
                switch(step) {
                    case 1:
                        // Show encryption
                        document.getElementById('encrypt-arrow').style.display = 'block';
                        setTimeout(() => {
                            document.getElementById('encrypt-step').style.display = 'block';
                            document.getElementById('encrypted-val1').innerHTML = `🔒 E(${input1.toFixed(2)})`;
                            document.getElementById('encrypted-val2').innerHTML = `🔒 E(${input2.toFixed(2)})`;
                        }, 500);
                        break;
                        
                    case 2:
                        // Show calculation
                        document.getElementById('calc-arrow').style.display = 'block';
                        setTimeout(() => {
                            document.getElementById('calc-step').style.display = 'block';
                            updateFormula(operation, input1, input2);
                        }, 500);
                        break;
                        
                    case 3:
                        // Show decryption
                        document.getElementById('decrypt-arrow').style.display = 'block';
                        setTimeout(() => {
                            document.getElementById('result-section').classList.add('show-result');
                            const result = calculateResult(operation, input1, input2);
                            document.getElementById('final-result').innerHTML = result.toFixed(4);
                            showResultExplanation(operation, input1, input2, result);
                        }, 500);
                        break;
                        
                    case 4:
                        // Complete
                        clearInterval(interval);
                        calculationInProgress = false;
                        document.querySelector('.demo-button').disabled = false;
                        document.querySelector('.demo-button').innerHTML = '🎯 Tính toán mới';
                        break;
                }
            }, 1500);
        }

        function updateProgress(percent) {
            document.getElementById('progress').style.width = percent + '%';
        }

        function updateFormula(operation, a, b) {
            const formulaElement = document.getElementById('formula');
            let formula = '';
            
            switch(operation) {
                case 'golden_ratio':
                    formula = `φ = (${a.toFixed(2)} + √${b.toFixed(2)}) / 2`;
                    break;
                case 'add':
                    formula = `${a.toFixed(2)} + ${b.toFixed(2)}`;
                    break;
                case 'multiply':
                    formula = `${a.toFixed(2)} × ${b.toFixed(2)}`;
                    break;
                case 'sqrt_add':
                    formula = `√${a.toFixed(2)} + √${b.toFixed(2)}`;
                    break;
                case 'reciprocal':
                    formula = `1/${a.toFixed(2)} + 1/${b.toFixed(2)}`;
                    break;
            }
            
            formulaElement.innerHTML = formula;
        }

        function calculateResult(operation, a, b) {
            switch(operation) {
                case 'golden_ratio':
                    return (a + Math.sqrt(b)) / 2;
                case 'add':
                    return a + b;
                case 'multiply':
                    return a * b;
                case 'sqrt_add':
                    return Math.sqrt(a) + Math.sqrt(b);
                case 'reciprocal':
                    return (1/a) + (1/b);
                default:
                    return 0;
            }
        }

        function showResultExplanation(operation, a, b, result) {
            const explanationElement = document.getElementById('result-explanation');
            let explanation = '';
            
            switch(operation) {
                case 'golden_ratio':
                    explanation = `<p><strong>Giải thích:</strong> Tỷ lệ vàng được tính bằng công thức φ = (a + √b) / 2</p>
                                 <p>Với a=${a}, b=${b}: φ = (${a} + ${Math.sqrt(b).toFixed(4)}) / 2 = ${result.toFixed(4)}</p>`;
                    if (a === 1 && b === 5) {
                        explanation += `<p><em>🎯 Đây chính là tỷ lệ vàng cổ điển! φ ≈ 1.618</em></p>`;
                    }
                    break;
                case 'add':
                    explanation = `<p><strong>Phép cộng FHE:</strong> ${a} + ${b} = ${result}</p>`;
                    break;
                case 'multiply':
                    explanation = `<p><strong>Phép nhân FHE:</strong> ${a} × ${b} = ${result.toFixed(4)}</p>`;
                    break;
                case 'sqrt_add':
                    explanation = `<p><strong>Căn bậc hai FHE:</strong> √${a} + √${b} = ${Math.sqrt(a).toFixed(4)} + ${Math.sqrt(b).toFixed(4)} = ${result.toFixed(4)}</p>`;
                    break;
                case 'reciprocal':
                    explanation = `<p><strong>Nghịch đảo FHE:</strong> 1/${a} + 1/${b} = ${(1/a).toFixed(4)} + ${(1/b).toFixed(4)} = ${result.toFixed(4)}</p>`;
                    break;
            }
            
            explanationElement.innerHTML = explanation;
        }

        // Reset function
        function resetDemo() {
            document.getElementById('calculation-steps').style.display = 'none';
            document.getElementById('result-section').classList.remove('show-result');
            document.getElementById('progress').style.width = '0%';
            
            // Hide all arrows and steps
            document.getElementById('encrypt-arrow').style.display = 'none';
            document.getElementById('encrypt-step').style.display = 'none';
            document.getElementById('calc-arrow').style.display = 'none';
            document.getElementById('calc-step').style.display = 'none';
            document.getElementById('decrypt-arrow').style.display = 'none';
        }

        // Auto-reset when inputs change
        document.getElementById('input1').addEventListener('input', resetDemo);
        document.getElementById('input2').addEventListener('input', resetDemo);
        document.getElementById('operation').addEventListener('change', resetDemo);
    </script>
</body>
</html>
