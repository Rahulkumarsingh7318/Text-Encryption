# Text-Encryption
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CyberVault - Secure Text Encryption</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .cipher-container {
            transition: all 0.3s ease;
        }
        .cipher-container:hover {
            box-shadow: 0 10px 25px rgba(0, 153, 255, 0.2);
        }
        .security-meter {
            width: 100%;
            height: 8px;
            border-radius: 4px;
            background: linear-gradient(to right, #ff0000, #ffff00, #00ff00);
            margin-top: 8px;
        }
        .security-marker {
            width: 10px;
            height: 12px;
            background-color: white;
            border: 2px solid #333;
            border-radius: 3px;
            position: absolute;
            top: -10px;
            transform: translateX(-50%);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">

    <div class="container mx-auto px-4 py-12">
        <!-- Header Section -->
        <header class="mb-12 text-center">
            <div class="flex justify-center mb-6">
                <img src="https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/9a0c16f7-347e-4232-8f63-4bb609f778a8.png" alt="CyberVault logo featuring a futuristic shield with binary code background in blue and black colors" class="w-24 h-24" />
            </div>
            <h1 class="text-4xl font-bold text-gray-800 mb-2">CyberVault</h1>
            <p class="text-lg text-gray-600">Multi-algorithm text encryption for maximum security</p>
        </header>

        <!-- Main Encryption Interface -->
        <div class="max-w-3xl mx-auto bg-white rounded-xl shadow-md overflow-hidden cipher-container">
            <div class="p-8">
                <div class="mb-6">
                    <label for="plaintext" class="block text-sm font-medium text-gray-700 mb-2">Text to Encrypt</label>
                    <textarea id="plaintext" rows="5" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Enter your secret text here..."></textarea>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                    <!-- Algorithm Selection -->
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-2">Encryption Algorithm</label>
                        <select id="algorithm" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="AES-256-CBC">AES-256 (Strongest)</option>
                            <option value="AES-128-CBC">AES-128</option>
                            <option value="DES-CBC">DES (Legacy)</option>
                            <option value="RSA-OAEP">RSA (Asymmetric)</option>
                        </select>
                        <div class="mt-2 relative">
                            <div class="security-meter">
                                <div id="security-level" class="security-marker" style="left: 90%;"></div>
                            </div>
                            <div class="flex justify-between text-xs text-gray-500 mt-1">
                                <span>Basic</span>
                                <span>Standard</span>
                                <span>Military</span>
                            </div>
                        </div>
                    </div>

                    <!-- Key Input -->
                    <div id="symmetric-key-container">
                        <label for="key" class="block text-sm font-medium text-gray-700 mb-2">Encryption Key</label>
                        <input type="text" id="key" class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Enter your secret key">
                    </div>

                    <!-- RSA Key Generation -->
                    <div id="rsa-options-container" class="hidden">
                        <label class="block text-sm font-medium text-gray-700 mb-2">RSA Options</label>
                        <button id="generate-keys" class="w-full bg-blue-600 hover:bg-blue-700 text-white py-2 px-4 rounded-md transition">Generate Key Pair</button>
                        <div class="mt-2 hidden" id="key-status">
                            <p class="text-xs text-green-600">Key pair generated!</p>
                        </div>
                    </div>
                </div>

                <button id="encrypt-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white py-3 px-4 rounded-md font-medium transition">
                    Encrypt
                </button>
            </div>
        </div>

        <!-- Results Section -->
        <div id="results" class="max-w-3xl mx-auto mt-8 bg-white rounded-xl shadow-md overflow-hidden cipher-container hidden">
            <div class="p-8">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Encryption Results</h2>
                
                <!-- Encrypted Output -->
                <div class="mb-6">
                    <label class="block text-sm font-medium text-gray-700 mb-2">Encrypted Text</label>
                    <div class="relative">
                        <textarea id="ciphertext" rows="5" class="w-full px-3 py-2 border border-gray-300 rounded-md bg-gray-50" readonly></textarea>
                        <button id="copy-btn" class="absolute right-2 top-2 bg-gray-200 hover:bg-gray-300 text-gray-800 py-1 px-2 rounded-md text-sm transition">Copy</button>
                    </div>
                </div>

                <!-- Additional Info -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <p class="text-sm text-gray-700 mb-1">Algorithm Used:</p>
                        <p id="algorithm-used" class="font-medium text-gray-900">-</p>
                    </div>
                    <div>
                        <p class="text-sm text-gray-700 mb-1">Encryption Time:</p>
                        <p id="encryption-time" class="font-medium text-gray-900">-</p>
                    </div>
                </div>

                <!-- Security Rating -->
                <div class="mt-6">
                    <p class="text-sm text-gray-700 mb-2">Security Rating:</p>
                    <div class="flex items-center">
                        <div class="w-4 h-4 rounded-full mr-2" id="security-badge"></div>
                        <p id="security-rating" class="text-sm font-medium">-</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Algorithm Info Section -->
        <div class="max-w-3xl mx-auto mt-12">
            <h2 class="text-2xl font-bold text-gray-800 mb-6">Algorithm Security Information</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- AES Card -->
                <div class="bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="p-4 bg-blue-600">
                        <h3 class="text-lg font-semibold text-white">AES-256</h3>
                    </div>
                    <div class="p-6">
                        <p class="text-sm text-gray-600 mb-4">Advanced Encryption Standard with 256-bit keys. The gold standard for symmetric encryption used by governments worldwide.</p>
                        <div class="flex justify-between items-center">
                            <span class="text-xs font-medium px-2 py-1 bg-blue-100 text-blue-800 rounded-full">Military Grade</span>
                        </div>
                    </div>
                </div>

                <!-- DES Card -->
                <div class="bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="p-4 bg-yellow-500">
                        <h3 class="text-lg font-semibold text-white">DES</h3>
                    </div>
                    <div class="p-6">
                        <p class="text-sm text-gray-600 mb-4">Data Encryption Standard (56-bit). Legacy algorithm that can be broken with modern hardware. Not recommended for sensitive data.</p>
                        <div class="flex justify-between items-center">
                            <span class="text-xs font-medium px-2 py-1 bg-yellow-100 text-yellow-800 rounded-full">Legacy</span>
                        </div>
                    </div>
                </div>

                <!-- RSA Card -->
                <div class="bg-white rounded-lg shadow-md overflow-hidden">
                    <div class="p-4 bg-green-600">
                        <h3 class="text-lg font-semibold text-white">RSA</h3>
                    </div>
                    <div class="p-6">
                        <p class="text-sm text-gray-600 mb-4">Public-key algorithm (2048-bit here). Asymmetric encryption ideal for key exchange and digital signatures.</p>
                        <div class="flex justify-between items-center">
                            <span class="text-xs font-medium px-2 py-1 bg-green-100 text-green-800 rounded-full">Secure</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Scripts section with encryption logic -->
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM elements
            const algorithmSelect = document.getElementById('algorithm');
            const encryptBtn = document.getElementById('encrypt-btn');
            const plaintext = document.getElementById('plaintext');
            const ciphertext = document.getElementById('ciphertext');
            const copyBtn = document.getElementById('copy-btn');
            const results = document.getElementById('results');
            const symmetricKeyContainer = document.getElementById('symmetric-key-container');
            const rsaOptionsContainer = document.getElementById('rsa-options-container');
            const generateKeysBtn = document.getElementById('generate-keys');
            const keyStatus = document.getElementById('key-status');
            const securityLevel = document.getElementById('security-level');
            const securityBadge = document.getElementById('security-badge');
            const securityRating = document.getElementById('security-rating');
            const algorithmUsed = document.getElementById('algorithm-used');
            const encryptionTime = document.getElementById('encryption-time');
            
            // For RSA keys storage
            let rsaKeys = {
                publicKey: null,
                privateKey: null
            };
            
            // Update UI based on algorithm selection
            algorithmSelect.addEventListener('change', function() {
                const value = this.value;
                
                // Update security level marker
                if (value === 'AES-256-CBC') {
                    securityLevel.style.left = '90%';
                } else if (value === 'AES-128-CBC') {
                    securityLevel.style.left = '75%';
                } else if (value === 'DES-CBC') {
                    securityLevel.style.left = '25%';
                } else if (value === 'RSA-OAEP') {
                    securityLevel.style.left = '80%';
                }
                
                // Toggle between symmetric and asymmetric options
                if (value === 'RSA-OAEP') {
                    symmetricKeyContainer.classList.add('hidden');
                    rsaOptionsContainer.classList.remove('hidden');
                } else {
                    symmetricKeyContainer.classList.remove('hidden');
                    rsaOptionsContainer.classList.add('hidden');
                }
            });
            
            // Generate RSA keys
            generateKeysBtn.addEventListener('click', async function() {
                try {
                    const keyPair = await generateRSAKeyPair();
                    rsaKeys = {
                        publicKey: keyPair.publicKey,
                        privateKey: keyPair.privateKey
                    };
                    keyStatus.classList.remove('hidden');
                    generateKeysBtn.textContent = 'Regenerate Keys';
                } catch (error) {
                    console.error('Key generation failed:', error);
                }
            });
            
            // Copy to clipboard
            copyBtn.addEventListener('click', function() {
                ciphertext.select();
                document.execCommand('copy');
                const originalText = this.textContent;
                this.textContent = 'Copied!';
                setTimeout(() => {
                    this.textContent = originalText;
                }, 2000);
            });
            
            // Main encryption handler
            encryptBtn.addEventListener('click', async function() {
                const algorithm = algorithmSelect.value;
                const inputText = plaintext.value.trim();
                
                if (!inputText) {
                    alert('Please enter text to encrypt');
                    return;
                }
                
                try {
                    let encryptedText;
                    const startTime = performance.now();
                    
                    if (algorithm === 'RSA-OAEP') {
                        if (!rsaKeys.publicKey) {
                            alert('Please generate RSA keys first');
                            return;
                        }
                        encryptedText = await encryptRSA(inputText, rsaKeys.publicKey);
                    } else {
                        const key = document.getElementById('key').value.trim();
                        if (!key) {
                            alert('Please enter an encryption key');
                            return;
                        }
                        encryptedText = await encryptSymmetric(inputText, key, algorithm);
                    }
                    
                    const endTime = performance.now();
                    const encryptionDuration = (endTime - startTime).toFixed(2);
                    
                    // Display results
                    ciphertext.value = encryptedText;
                    algorithmUsed.textContent = algorithm;
                    encryptionTime.textContent = `${encryptionDuration} ms`;
                    
                    // Set security rating
                    setSecurityRating(algorithm);
                    
                    results.classList.remove('hidden');
                } catch (error) {
                    console.error('Encryption failed:', error);
                    alert('Encryption failed. See console for details.');
                }
            });
            
            // Set security rating UI
            function setSecurityRating(algorithm) {
                if (algorithm === 'AES-256-CBC') {
                    securityBadge.className = 'w-4 h-4 rounded-full mr-2 bg-green-500';
                    securityRating.textContent = 'Military Grade (Extremely Secure)';
                } else if (algorithm === 'AES-128-CBC') {
                    securityBadge.className = 'w-4 h-4 rounded-full mr-2 bg-blue-500';
                    securityRating.textContent = 'Enterprise Grade (Very Secure)';
                } else if (algorithm === 'DES-CBC') {
                    securityBadge.className = 'w-4 h-4 rounded-full mr-2 bg-yellow-500';
                    securityRating.textContent = 'Legacy (Not Recommended)';
                } else if (algorithm === 'RSA-OAEP') {
                    securityBadge.className = 'w-4 h-4 rounded-full mr-2 bg-green-500';
                    securityRating.textContent = 'Secure (Asymmetric)';
                }
            }
            
            // Encryption functions
            async function generateRSAKeyPair() {
                return await window.crypto.subtle.generateKey(
                    {
                        name: "RSA-OAEP",
                        modulusLength: 2048,
                        publicExponent: new Uint8Array([1, 0, 1]),
                        hash: "SHA-256"
                    },
                    true,
                    ["encrypt", "decrypt"]
                );
            }
            
            async function encryptRSA(text, publicKey) {
                const encoder = new TextEncoder();
                const encoded = encoder.encode(text);
                
                // RSA has limited capacity - we need to chunk the input
                const chunkSize = 190; // 2048-bit RSA can encrypt ~245 bytes, but we're being conservative
                const chunks = [];
                
                for (let i = 0; i < encoded.length; i += chunkSize) {
                    const chunk = encoded.slice(i, i + chunkSize);
                    const encryptedChunk = await window.crypto.subtle.encrypt(
                        { name: "RSA-OAEP" },
                        publicKey,
                        chunk
                    );
                    chunks.push(encryptedChunk);
                }
                
                // Combine chunks and convert to Base64
                const combined = new Uint8Array(chunks.reduce((acc, chunk) => acc + chunk.byteLength, 0));
                let offset = 0;
                for (const chunk of chunks) {
                    combined.set(new Uint8Array(chunk), offset);
                    offset += chunk.byteLength;
                }
                
                return arrayBufferToBase64(combined);
            }
            
            async function encryptSymmetric(text, keyString, algorithmName) {
                const encoder = new TextEncoder();
                const data = encoder.encode(text);
                
                // Import the key
                let algorithmParams;
                let cryptoAlgorithm;
                
                if (algorithmName.startsWith('AES')) {
                    cryptoAlgorithm = { name: "AES-CBC" };
                    // Convert the key string to a suitable format
                    // For simplicity, we'll hash the user's input key to get 128/256 bits
                    const keyMaterial = await window.crypto.subtle.importKey(
                        "raw",
                        encoder.encode(keyString),
                        { name: "PBKDF2" },
                        false,
                        ["deriveKey", "deriveBits"]
                    );
                    
                    const salt = window.crypto.getRandomValues(new Uint8Array(16));
                    const key = await window.crypto.subtle.deriveKey(
                        {
                            name: "PBKDF2",
                            salt: salt,
                            iterations: 100000,
                            hash: "SHA-256"
                        },
                        keyMaterial,
                        { name: "AES-CBC", length: algorithmName === 'AES-256-CBC' ? 256 : 128 },
                        true,
                        ["encrypt", "decrypt"]
                    );
                    
                    const iv = window.crypto.getRandomValues(new Uint8Array(16));
                    const encrypted = await window.crypto.subtle.encrypt(
                        {
                            name: "AES-CBC",
                            iv: iv
                        },
                        key,
                        data
                    );
                    
                    // Combine salt + iv + ciphertext for decryption
                    const combined = new Uint8Array(salt.length + iv.length + encrypted.byteLength);
                    combined.set(salt, 0);
                    combined.set(iv, salt.length);
                    combined.set(new Uint8Array(encrypted), salt.length + iv.length);
                    
                    return arrayBufferToBase64(combined);
                } else if (algorithmName === 'DES-CBC') {
                    // DES implementation (since Web Crypto doesn't support DES natively)
                    return encryptDES(text, keyString);
                }
            }
            
            // Helper function to convert array buffer to Base64
            function arrayBufferToBase64(buffer) {
                let binary = '';
                const bytes = new Uint8Array(buffer);
                const len = bytes.byteLength;
                for (let i = 0; i < len; i++) {
                    binary += String.fromCharCode(bytes[i]);
                }
                return window.btoa(binary);
            }
            
            // DES implementation (since Web Crypto API doesn't support DES)
            function encryptDES(text, key) {
                // This is a simplified DES implementation for demonstration
                // Note: In a real application, you should not implement crypto yourself!
                // This is just to show how it might work if Web Crypto supported DES
                
                const encoder = new TextEncoder();
                const data = encoder.encode(text);
                const keyBytes = encoder.encode(key.substring(0, 8)); // DES uses 56-bit keys
                
                // This would be replaced with actual DES implementation
                console.warn('Using simulated DES encryption - not cryptographically secure!');
                
                // For demo purposes, we'll just XOR the data with the key repeatedly
                const encrypted = new Uint8Array(data.length);
                for (let i = 0; i < data.length; i++) {
                    encrypted[i] = data[i] ^ keyBytes[i % keyBytes.length];
                }
                
                return arrayBufferToBase64(encrypted);
            }
        });
    </script>
</body>
</html>

