<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Dados Pessoais</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    <style>
        .copy-btn {
            transition: all 0.3s ease;
        }
        .copy-btn:hover {
            transform: scale(1.05);
        }
        .copy-btn.copied {
            background-color: #10B981 !important;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .card {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            transition: all 0.3s ease;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">
    <div class="container mx-auto px-4 py-8">
        <header class="text-center mb-12">
            <h1 class="text-4xl font-bold text-indigo-700 mb-2">Gerador de Dados Pessoais</h1>
            <p class="text-gray-600 max-w-2xl mx-auto">Gere dados pessoais completos e realistas para testes e desenvolvimento</p>
        </header>

        <div class="max-w-4xl mx-auto">
            <div class="bg-white rounded-xl shadow-md p-6 mb-8 card">
                <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
                    <div>
                        <h2 class="text-xl font-semibold text-gray-800">Configurações</h2>
                        <p class="text-gray-500">Personalize os dados que deseja gerar</p>
                    </div>
                    <div class="flex gap-3">
                        <button id="generate-btn" class="bg-indigo-600 hover:bg-indigo-700 text-white px-6 py-2 rounded-lg font-medium transition-colors flex items-center gap-2">
                            <i class="fas fa-sync-alt"></i> Gerar Dados
                        </button>
                        <button id="copy-all-btn" class="bg-gray-200 hover:bg-gray-300 text-gray-800 px-6 py-2 rounded-lg font-medium transition-colors flex items-center gap-2">
                            <i class="fas fa-copy"></i> Copiar Tudo
                        </button>
                    </div>
                </div>

                <div class="mt-6 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                    <div class="flex items-center">
                        <input type="checkbox" id="include-photo" class="mr-2 h-5 w-5 text-indigo-600 rounded" checked>
                        <label for="include-photo" class="text-gray-700">Incluir Foto</label>
                    </div>
                    <div class="flex items-center">
                        <input type="checkbox" id="include-address" class="mr-2 h-5 w-5 text-indigo-600 rounded" checked>
                        <label for="include-address" class="text-gray-700">Endereço Completo</label>
                    </div>
                    <div class="flex items-center">
                        <input type="checkbox" id="include-documents" class="mr-2 h-5 w-5 text-indigo-600 rounded" checked>
                        <label for="include-documents" class="text-gray-700">Documentos (RG, CPF)</label>
                    </div>
                    <div class="flex items-center">
                        <input type="checkbox" id="include-contact" class="mr-2 h-5 w-5 text-indigo-600 rounded" checked>
                        <label for="include-contact" class="text-gray-700">Contatos</label>
                    </div>
                    <div class="flex items-center">
                        <input type="checkbox" id="include-bank" class="mr-2 h-5 w-5 text-indigo-600 rounded">
                        <label for="include-bank" class="text-gray-700">Dados Bancários</label>
                    </div>
                    <div class="flex items-center">
                        <input type="checkbox" id="include-professional" class="mr-2 h-5 w-5 text-indigo-600 rounded">
                        <label for="include-professional" class="text-gray-700">Dados Profissionais</label>
                    </div>
                </div>
            </div>

            <div id="result-container" class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Dados serão inseridos aqui via JavaScript -->
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Elementos do DOM
            const generateBtn = document.getElementById('generate-btn');
            const copyAllBtn = document.getElementById('copy-all-btn');
            const resultContainer = document.getElementById('result-container');
            
            // Configurações
            const includePhoto = document.getElementById('include-photo');
            const includeAddress = document.getElementById('include-address');
            const includeDocuments = document.getElementById('include-documents');
            const includeContact = document.getElementById('include-contact');
            const includeBank = document.getElementById('include-bank');
            const includeProfessional = document.getElementById('include-professional');

            // Arrays de dados para gerar informações realistas
            const firstNames = ['João', 'Maria', 'Pedro', 'Ana', 'Carlos', 'Mariana', 'Lucas', 'Juliana', 'Fernando', 'Patrícia'];
            const lastNames = ['Silva', 'Santos', 'Oliveira', 'Souza', 'Rodrigues', 'Ferreira', 'Alves', 'Pereira', 'Gomes', 'Martins'];
            const cities = ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte', 'Porto Alegre', 'Curitiba', 'Salvador', 'Recife', 'Fortaleza', 'Brasília', 'Manaus'];
            const states = ['SP', 'RJ', 'MG', 'RS', 'PR', 'BA', 'PE', 'CE', 'DF', 'AM'];
            const streets = ['Rua das Flores', 'Avenida Brasil', 'Rua São João', 'Avenida Paulista', 'Rua XV de Novembro', 'Avenida Getúlio Vargas', 'Rua da Paz', 'Avenida Rio Branco'];
            const occupations = ['Engenheiro', 'Médico', 'Professor', 'Advogado', 'Designer', 'Programador', 'Enfermeiro', 'Contador', 'Administrador', 'Vendedor'];
            const companies = ['Tech Solutions', 'Global Corp', 'Inova Sistemas', 'Mega Store', 'Consultoria ABC', 'Indústria XYZ', 'Serviços Integrados', 'Comércio Ltda'];
            const banks = ['Banco do Brasil', 'Itaú', 'Bradesco', 'Santander', 'Caixa Econômica', 'Nubank', 'Inter', 'Sicoob'];

            // Função para gerar um número aleatório no formato de CPF
            function generateCPF() {
                let cpf = '';
                for (let i = 0; i < 9; i++) {
                    cpf += Math.floor(Math.random() * 10);
                }
                
                // Cálculo do primeiro dígito verificador
                let sum = 0;
                for (let i = 0; i < 9; i++) {
                    sum += parseInt(cpf.charAt(i)) * (10 - i);
                }
                let firstDigit = (sum % 11 < 2) ? 0 : 11 - (sum % 11);
                cpf += firstDigit;
                
                // Cálculo do segundo dígito verificador
                sum = 0;
                for (let i = 0; i < 10; i++) {
                    sum += parseInt(cpf.charAt(i)) * (11 - i);
                }
                let secondDigit = (sum % 11 < 2) ? 0 : 11 - (sum % 11);
                cpf += secondDigit;
                
                return cpf.replace(/(\d{3})(\d{3})(\d{3})(\d{2})/, '$1.$2.$3-$4');
            }

            // Função para gerar um número aleatório no formato de RG
            function generateRG() {
                let rg = '';
                for (let i = 0; i < 8; i++) {
                    rg += Math.floor(Math.random() * 10);
                }
                return rg.replace(/(\d{2})(\d{3})(\d{3})(\d{1})/, '$1.$2.$3-$4');
            }

            // Função para gerar um número de telefone aleatório
            function generatePhone() {
                const ddd = ['11', '21', '31', '41', '51', '61', '71', '81', '91'];
                const randomDDD = ddd[Math.floor(Math.random() * ddd.length)];
                let number = '';
                for (let i = 0; i < 8; i++) {
                    number += Math.floor(Math.random() * 10);
                }
                return `(${randomDDD}) 9${number.substring(0, 4)}-${number.substring(4)}`;
            }

            // Função para gerar um número de conta bancária aleatório
            function generateBankAccount() {
                let account = '';
                for (let i = 0; i < 6; i++) {
                    account += Math.floor(Math.random() * 10);
                }
                return `${account}-${Math.floor(Math.random() * 10)}`;
            }

            // Função para gerar um número de agência bancária aleatório
            function generateBankAgency() {
                let agency = '';
                for (let i = 0; i < 4; i++) {
                    agency += Math.floor(Math.random() * 10);
                }
                return agency;
            }

            // Função para gerar dados pessoais
            function generatePerson() {
                const gender = Math.random() > 0.5 ? 'male' : 'female';
                const firstName = gender === 'male' 
                    ? firstNames.filter((_, i) => i % 2 === 0)[Math.floor(Math.random() * 5)]
                    : firstNames.filter((_, i) => i % 2 !== 0)[Math.floor(Math.random() * 5)];
                const lastName = lastNames[Math.floor(Math.random() * lastNames.length)];
                const fullName = `${firstName} ${lastName}`;
                
                const birthDate = new Date();
                birthDate.setFullYear(birthDate.getFullYear() - Math.floor(Math.random() * 50) - 18);
                birthDate.setMonth(Math.floor(Math.random() * 12));
                birthDate.setDate(Math.floor(Math.random() * 28) + 1);
                
                const formattedBirthDate = birthDate.toLocaleDateString('pt-BR');
                const age = new Date().getFullYear() - birthDate.getFullYear();
                
                const cityIndex = Math.floor(Math.random() * cities.length);
                const city = cities[cityIndex];
                const state = states[cityIndex];
                const street = streets[Math.floor(Math.random() * streets.length)];
                const number = Math.floor(Math.random() * 500) + 1;
                const zipCode = `${Math.floor(10000 + Math.random() * 90000)}-${Math.floor(100 + Math.random() * 900)}`;
                
                const email = `${firstName.toLowerCase()}.${lastName.toLowerCase()}@${['gmail.com', 'hotmail.com', 'outlook.com', 'yahoo.com.br'][Math.floor(Math.random() * 4)]}`;
                
                const cpf = generateCPF();
                const rg = generateRG();
                const phone = generatePhone();
                const cellphone = generatePhone();
                
                // Dados profissionais
                const occupation = occupations[Math.floor(Math.random() * occupations.length)];
                const company = companies[Math.floor(Math.random() * companies.length)];
                const salary = (Math.floor(Math.random() * 30) + 5) * 1000;
                
                // Dados bancários
                const bank = banks[Math.floor(Math.random() * banks.length)];
                const agency = generateBankAgency();
                const account = generateBankAccount();
                
                return {
                    photo: `https://randomuser.me/api/portraits/${gender === 'male' ? 'men' : 'women'}/${Math.floor(Math.random() * 100)}.jpg`,
                    fullName,
                    firstName,
                    lastName,
                    gender,
                    birthDate: formattedBirthDate,
                    age,
                    cpf,
                    rg,
                    email,
                    phone,
                    cellphone,
                    address: {
                        street,
                        number,
                        city,
                        state,
                        zipCode,
                        fullAddress: `${street}, ${number} - ${city}/${state}, CEP: ${zipCode}`
                    },
                    professional: {
                        occupation,
                        company,
                        salary: salary.toLocaleString('pt-BR', {style: 'currency', currency: 'BRL'})
                    },
                    bank: {
                        name: bank,
                        agency,
                        account,
                        fullBankInfo: `${bank} - Ag: ${agency} - CC: ${account}`
                    }
                };
            }

            // Função para copiar texto para a área de transferência
            function copyToClipboard(text, button) {
                navigator.clipboard.writeText(text).then(() => {
                    const originalText = button.innerHTML;
                    button.innerHTML = '<i class="fas fa-check mr-1"></i> Copiado!';
                    button.classList.add('copied');
                    
                    setTimeout(() => {
                        button.innerHTML = originalText;
                        button.classList.remove('copied');
                    }, 2000);
                });
            }

            // Função para copiar todos os dados
            function copyAllData() {
                let allText = '';
                const cards = document.querySelectorAll('.data-card');
                
                cards.forEach(card => {
                    const title = card.querySelector('h3').textContent;
                    const content = card.querySelector('.data-content').textContent;
                    allText += `${title}\n${content}\n\n`;
                });
                
                copyToClipboard(allText.trim(), copyAllBtn);
            }

            // Função para renderizar os dados gerados
            function renderPersonData(person) {
                resultContainer.innerHTML = '';
                
                // Card de informações básicas
                let basicInfoHTML = `
                    <div class="bg-white rounded-xl p-6 card fade-in data-card">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-lg font-semibold text-gray-800 flex items-center gap-2">
                                <i class="fas fa-user text-indigo-500"></i> Informações Pessoais
                            </h3>
                            <button class="copy-btn bg-indigo-100 text-indigo-600 px-3 py-1 rounded text-sm flex items-center gap-1">
                                <i class="fas fa-copy"></i> Copiar
                            </button>
                        </div>
                        <div class="data-content">
                            <div class="flex flex-col md:flex-row gap-4 mb-4">
                                ${includePhoto.checked ? `
                                <div class="flex-shrink-0">
                                    <img src="${person.photo}" alt="Foto" class="w-24 h-24 rounded-full object-cover border-2 border-indigo-100">
                                </div>
                                ` : ''}
                                <div>
                                    <p class="text-gray-700"><span class="font-medium">Nome:</span> ${person.fullName}</p>
                                    <p class="text-gray-700"><span class="font-medium">Sexo:</span> ${person.gender === 'male' ? 'Masculino' : 'Feminino'}</p>
                                    <p class="text-gray-700"><span class="font-medium">Data de Nascimento:</span> ${person.birthDate}</p>
                                    <p class="text-gray-700"><span class="font-medium">Idade:</span> ${person.age} anos</p>
                                    ${includeDocuments.checked ? `
                                    <p class="text-gray-700"><span class="font-medium">CPF:</span> ${person.cpf}</p>
                                    <p class="text-gray-700"><span class="font-medium">RG:</span> ${person.rg}</p>
                                    ` : ''}
                                </div>
                            </div>
                        </div>
                    </div>
                `;
                
                // Card de contato
                let contactHTML = `
                    <div class="bg-white rounded-xl p-6 card fade-in data-card">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-lg font-semibold text-gray-800 flex items-center gap-2">
                                <i class="fas fa-address-book text-indigo-500"></i> Contato
                            </h3>
                            <button class="copy-btn bg-indigo-100 text-indigo-600 px-3 py-1 rounded text-sm flex items-center gap-1">
                                <i class="fas fa-copy"></i> Copiar
                            </button>
                        </div>
                        <div class="data-content">
                            ${includeContact.checked ? `
                            <p class="text-gray-700 mb-2"><span class="font-medium">E-mail:</span> ${person.email}</p>
                            <p class="text-gray-700 mb-2"><span class="font-medium">Telefone:</span> ${person.phone}</p>
                            <p class="text-gray-700"><span class="font-medium">Celular:</span> ${person.cellphone}</p>
                            ` : 'Nenhuma informação de contato incluída'}
                        </div>
                    </div>
                `;
                
                // Card de endereço
                let addressHTML = includeAddress.checked ? `
                    <div class="bg-white rounded-xl p-6 card fade-in data-card">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-lg font-semibold text-gray-800 flex items-center gap-2">
                                <i class="fas fa-map-marker-alt text-indigo-500"></i> Endereço
                            </h3>
                            <button class="copy-btn bg-indigo-100 text-indigo-600 px-3 py-1 rounded text-sm flex items-center gap-1">
                                <i class="fas fa-copy"></i> Copiar
                            </button>
                        </div>
                        <div class="data-content">
                            <p class="text-gray-700"><span class="font-medium">Endereço:</span> ${person.address.fullAddress}</p>
                        </div>
                    </div>
                ` : '';
                
                // Card de dados profissionais
                let professionalHTML = includeProfessional.checked ? `
                    <div class="bg-white rounded-xl p-6 card fade-in data-card">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-lg font-semibold text-gray-800 flex items-center gap-2">
                                <i class="fas fa-briefcase text-indigo-500"></i> Dados Profissionais
                            </h3>
                            <button class="copy-btn bg-indigo-100 text-indigo-600 px-3 py-1 rounded text-sm flex items-center gap-1">
                                <i class="fas fa-copy"></i> Copiar
                            </button>
                        </div>
                        <div class="data-content">
                            <p class="text-gray-700"><span class="font-medium">Profissão:</span> ${person.professional.occupation}</p>
                            <p class="text-gray-700"><span class="font-medium">Empresa:</span> ${person.professional.company}</p>
                            <p class="text-gray-700"><span class="font-medium">Salário:</span> ${person.professional.salary}</p>
                        </div>
                    </div>
                ` : '';
                
                // Card de dados bancários
                let bankHTML = includeBank.checked ? `
                    <div class="bg-white rounded-xl p-6 card fade-in data-card">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="text-lg font-semibold text-gray-800 flex items-center gap-2">
                                <i class="fas fa-university text-indigo-500"></i> Dados Bancários
                            </h3>
                            <button class="copy-btn bg-indigo-100 text-indigo-600 px-3 py-1 rounded text-sm flex items-center gap-1">
                                <i class="fas fa-copy"></i> Copiar
                            </button>
                        </div>
                        <div class="data-content">
                            <p class="text-gray-700"><span class="font-medium">Banco:</span> ${person.bank.fullBankInfo}</p>
                        </div>
                    </div>
                ` : '';
                
                // Adiciona todos os cards ao container
                resultContainer.innerHTML += basicInfoHTML;
                resultContainer.innerHTML += contactHTML;
                if (includeAddress.checked) resultContainer.innerHTML += addressHTML;
                if (includeProfessional.checked) resultContainer.innerHTML += professionalHTML;
                if (includeBank.checked) resultContainer.innerHTML += bankHTML;
                
                // Adiciona eventos de clique aos botões de copiar
                document.querySelectorAll('.copy-btn').forEach(btn => {
                    btn.addEventListener('click', function() {
                        const card = this.closest('.data-card');
                        const content = card.querySelector('.data-content').textContent;
                        copyToClipboard(content.trim(), this);
                    });
                });
            }

            // Event listeners
            generateBtn.addEventListener('click', function() {
                const person = generatePerson();
                renderPersonData(person);
            });
            
            copyAllBtn.addEventListener('click', copyAllData);
            
            // Gerar dados automaticamente ao carregar a página
            generateBtn.click();
        });
    </script>
</body>
</html>
