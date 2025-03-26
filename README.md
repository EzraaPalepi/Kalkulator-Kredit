# Kalkulator-Kredit
Kalkulator Kredit Online
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulator Kredit BPR Penataran</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
    <style>
        .card-shadow {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
        }
        .bg-gradient {
            background: linear-gradient(135deg, #f0f9ff 0%, #e0e7ff 100%);
        }
        .summary-table {
            width: 100%;
            border-collapse: collapse;
        }
        .summary-table th, .summary-table td {
            border: 1px solid #e2e8f0;
            padding: 8px 12px;
        }
        .summary-table th {
            background-color: #f8fafc;
            font-weight: 600;
            text-align: left;
        }
        .summary-table td {
            text-align: right;
        }
        .number-cell {
            text-align: right;
            font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
        }
        @media (max-width: 640px) {
            .mobile-card {
                padding: 1rem;
            }
            .mobile-input {
                padding: 0.5rem;
                font-size: 0.875rem;
            }
            .mobile-table th, .mobile-table td {
                padding: 0.5rem;
                font-size: 0.75rem;
            }
            .mobile-btn {
                padding: 0.75rem;
                font-size: 0.875rem;
            }
        }
    </style>
</head>
<body class="bg-gradient min-h-screen p-2 sm:p-4">
    <div class="container mx-auto">
        <!-- Main Card -->
        <div class="card-shadow bg-white rounded-lg max-w-4xl mx-auto">
            <!-- Card Header -->
            <div class="bg-indigo-600 rounded-t-lg p-3 sm:p-4">
                <h2 class="text-xl sm:text-2xl text-center text-white font-bold">Simulator Kredit BPR Penataran</h2>
            </div>
            
            <!-- Card Content -->
            <div class="p-3 sm:p-6">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-3 sm:gap-6">
                    <div class="space-y-1 sm:space-y-2">
                        <label class="block text-sm font-medium text-gray-700">Jumlah Pinjaman (Rp)</label>
                        <input type="text" id="pinjaman" placeholder="Contoh: 10.000.000" 
                               class="w-full p-2 mobile-input border rounded focus:ring-2 focus:ring-indigo-500 text-right">
                    </div>
                    <div class="space-y-1 sm:space-y-2">
                        <label class="block text-sm font-medium text-gray-700">Lama Pinjaman (Bulan)</label>
                        <input type="number" id="lamaPinjaman" placeholder="Contoh: 12" 
                               class="w-full p-2 mobile-input border rounded focus:ring-2 focus:ring-indigo-500 text-right">
                    </div>
                    <div class="space-y-1 sm:space-y-2">
                        <label class="block text-sm font-medium text-gray-700">Bunga Pertahun (%)</label>
                        <input type="number" id="bungaTahunan" placeholder="Contoh: 10" 
                               class="w-full p-2 mobile-input border rounded focus:ring-2 focus:ring-indigo-500 text-right">
                    </div>
                    <div class="space-y-1 sm:space-y-2">
                        <label class="block text-sm font-medium text-gray-700">Metode Perhitungan</label>
                        <select id="modePerhitungan" class="w-full p-2 mobile-input border rounded focus:ring-2 focus:ring-indigo-500">
                            <option value="flat">Flat</option>
                            <option value="anuitas">Anuitas</option>
                            <option value="efektif">Efektif</option>
                        </select>
                    </div>
                </div>
            </div>
            
            <!-- Card Footer -->
            <div class="bg-gray-50 rounded-b-lg p-3 sm:p-6">
                <button id="hitungBtn" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white py-2 sm:py-3 mobile-btn rounded font-semibold">
                    Hitung Simulasi
                </button>
            </div>
        </div>

        <!-- Results Container (Initially Hidden) -->
        <div id="resultsContainer" class="hidden mt-4 sm:mt-6 space-y-4 sm:space-y-6 max-w-4xl mx-auto">
            <!-- Ringkasan Perhitungan -->
            <div class="card-shadow bg-white rounded-lg">
                <div class="bg-indigo-600 rounded-t-lg p-3 sm:p-4">
                    <h2 class="text-xl sm:text-2xl text-center text-white font-bold">Ringkasan Perhitungan</h2>
                </div>
                <div class="bg-white p-3 sm:p-6 overflow-x-auto">
                    <table class="summary-table w-full">
                        <tbody>
                            <tr>
                                <th width="40%" class="text-sm sm:text-base">Metode Perhitungan</th>
                                <td id="metodePerhitungan" width="60%" class="text-sm sm:text-base">-</td>
                            </tr>
                            <tr>
                                <th class="text-sm sm:text-base">Jumlah Pinjaman</th>
                                <td id="jumlahPinjaman" class="number-cell text-sm sm:text-base">Rp 0</td>
                            </tr>
                            <tr>
                                <th class="text-sm sm:text-base">Lama Pinjaman</th>
                                <td id="lamaPinjamanSummary" class="number-cell text-sm sm:text-base">0 Bulan</td>
                            </tr>
                            <tr>
                                <th class="text-sm sm:text-base">Bunga Pertahun</th>
                                <td id="bungaTahunanSummary" class="number-cell text-sm sm:text-base">0%</td>
                            </tr>
                            <tr>
                                <th class="text-sm sm:text-base">Angsuran Bulanan</th>
                                <td id="angsuranBulanan" class="number-cell text-sm sm:text-base">Rp 0</td>
                            </tr>
                            <tr>
                                <th class="text-sm sm:text-base">Total Bunga</th>
                                <td id="totalBunga" class="number-cell text-sm sm:text-base">Rp 0</td>
                            </tr>
                            <tr>
                                <th class="text-sm sm:text-base">Total Pembayaran</th>
                                <td id="totalPembayaran" class="number-cell text-sm sm:text-base">Rp 0</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Tabel Angsuran -->
            <div class="card-shadow bg-white rounded-lg">
                <div class="bg-indigo-600 rounded-t-lg p-3 sm:p-4">
                    <h2 class="text-xl sm:text-2xl text-center text-white font-bold">Tabel Angsuran</h2>
                </div>
                <div class="overflow-x-auto p-0">
                    <table class="w-full border-collapse mobile-table" id="tabelAngsuran">
                        <thead>
                            <tr class="bg-indigo-100">
                                <th class="p-2 sm:p-3 border text-left text-indigo-800 font-semibold text-sm sm:text-base">Bulan</th>
                                <th class="p-2 sm:p-3 border text-right text-indigo-800 font-semibold text-sm sm:text-base">Pokok (Rp)</th>
                                <th class="p-2 sm:p-3 border text-right text-indigo-800 font-semibold text-sm sm:text-base">Bunga (Rp)</th>
                                <th class="p-2 sm:p-3 border text-right text-indigo-800 font-semibold text-sm sm:text-base">Total (Rp)</th>
                                <th class="p-2 sm:p-3 border text-right text-indigo-800 font-semibold text-sm sm:text-base">Sisa (Rp)</th>
                            </tr>
                        </thead>
                        <tbody id="tabelBody">
                            <!-- Will be filled by JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Download Button -->
            <div class="text-center pt-2">
                <button id="downloadPdf" class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 sm:px-6 sm:py-3 mobile-btn rounded-lg font-semibold">
                    Download PDF Laporan
                </button>
            </div>
        </div>
    </div>

    <script>
        // Initialize jsPDF
        const { jsPDF } = window.jspdf;
        
        // Format number with dots as thousand separators (without decimals)
        function formatNominal(value) {
            if (!value) return '0';
            return Math.round(value).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        }

        // Remove dots for calculations
        function parseNominal(value) {
            return value.replace(/\./g, '');
        }

        // Flat calculation method
        function hitungAngsuranFlat(pokok, bunga, tenor) {
            const angsuranBunga = (pokok * bunga) / tenor;
            const angsuranPokok = pokok / tenor;
            const totalAngsuran = angsuranPokok + angsuranBunga;
            const totalBunga = angsuranBunga * tenor;

            const tabel = [];
            let sisaPokok = pokok;

            for (let i = 1; i <= tenor; i++) {
                tabel.push({
                    bulan: i,
                    angsuranPokok: angsuranPokok,
                    angsuranBunga: angsuranBunga,
                    totalAngsuran: totalAngsuran,
                    sisaPokok: Math.max(0, sisaPokok - angsuranPokok)
                });
                sisaPokok -= angsuranPokok;
            }

            return {
                tabel: tabel,
                ringkasan: {
                    metodePerhitungan: 'Flat',
                    angsuranBulanan: totalAngsuran,
                    totalBunga: totalBunga,
                    pokok: pokok,
                    tenor: tenor,
                    bungaTahunan: bunga * 100,
                    totalPembayaran: pokok + totalBunga
                }
            };
        }

        // Anuitas calculation method
        function hitungAngsuranAnuitas(pokok, bunga, tenor) {
            const monthlyBunga = bunga / 12;
            const angsuranBulanan = pokok * (monthlyBunga * Math.pow(1 + monthlyBunga, tenor)) / (Math.pow(1 + monthlyBunga, tenor) - 1);
            let totalBunga = 0;

            const tabel = [];
            let sisaPokok = pokok;

            for (let i = 1; i <= tenor; i++) {
                const angsuranBunga = sisaPokok * monthlyBunga;
                const angsuranPokok = angsuranBulanan - angsuranBunga;
                totalBunga += angsuranBunga;

                tabel.push({
                    bulan: i,
                    angsuranPokok: angsuranPokok,
                    angsuranBunga: angsuranBunga,
                    totalAngsuran: angsuranBulanan,
                    sisaPokok: Math.max(0, sisaPokok - angsuranPokok)
                });

                sisaPokok -= angsuranPokok;
            }

            return {
                tabel: tabel,
                ringkasan: {
                    metodePerhitungan: 'Anuitas',
                    angsuranBulanan: angsuranBulanan,
                    totalBunga: totalBunga,
                    pokok: pokok,
                    tenor: tenor,
                    bungaTahunan: bunga * 100,
                    totalPembayaran: pokok + totalBunga
                }
            };
        }

        // Efektif calculation method
        function hitungAngsuranEfektif(pokok, bunga, tenor) {
            const monthlyBunga = bunga / 12;
            const angsuranPokok = pokok / tenor;
            let totalBunga = 0;

            const tabel = [];
            let sisaPokok = pokok;

            for (let i = 1; i <= tenor; i++) {
                const angsuranBunga = sisaPokok * monthlyBunga;
                totalBunga += angsuranBunga;
                const totalAngsuran = angsuranPokok + angsuranBunga;

                tabel.push({
                    bulan: i,
                    angsuranPokok: angsuranPokok,
                    angsuranBunga: angsuranBunga,
                    totalAngsuran: totalAngsuran,
                    sisaPokok: Math.max(0, sisaPokok - angsuranPokok)
                });

                sisaPokok -= angsuranPokok;
            }

            return {
                tabel: tabel,
                ringkasan: {
                    metodePerhitungan: 'Efektif',
                    angsuranBulanan: angsuranPokok + (pokok * monthlyBunga),
                    totalBunga: totalBunga,
                    pokok: pokok,
                    tenor: tenor,
                    bungaTahunan: bunga * 100,
                    totalPembayaran: pokok + totalBunga
                }
            };
        }

        // Generate PDF
        function generatePDF(result) {
            const doc = new jsPDF();
            
            // Add title and date
            doc.setFontSize(14);
            doc.setTextColor(40, 40, 40);
            doc.text('LAPORAN SIMULASI KREDIT BPR PENATARAN', 105, 15, { align: 'center' });
            
            doc.setFontSize(9);
            doc.setTextColor(100, 100, 100);
            doc.text(`Dibuat pada: ${new Date().toLocaleDateString('id-ID')}`, 105, 20, { align: 'center' });
            
            // Add summary table
            doc.setFontSize(9);
            doc.setTextColor(40, 40, 40);
            
            const summaryData = [
                ['Metode Perhitungan', result.ringkasan.metodePerhitungan],
                ['Jumlah Pinjaman', `Rp ${formatNominal(result.ringkasan.pokok)}`],
                ['Lama Pinjaman', `${result.ringkasan.tenor} Bulan`],
                ['Bunga Pertahun', `${Math.round(result.ringkasan.bungaTahunan)}%`],
                ['Angsuran Bulanan', `Rp ${formatNominal(result.ringkasan.angsuranBulanan)}`],
                ['Total Bunga', `Rp ${formatNominal(result.ringkasan.totalBunga)}`],
                ['Total Pembayaran', `Rp ${formatNominal(result.ringkasan.totalPembayaran)}`]
            ];
            
            doc.autoTable({
                startY: 25,
                head: [['Parameter', 'Nilai']],
                body: summaryData,
                theme: 'grid',
                headStyles: {
                    fillColor: [79, 70, 229],
                    textColor: 255,
                    fontStyle: 'bold'
                },
                styles: {
                    cellPadding: 3,
                    fontSize: 9,
                    valign: 'middle'
                },
                columnStyles: {
                    0: { fontStyle: 'bold', cellWidth: 50, halign: 'left' },
                    1: { cellWidth: 'auto', halign: 'right' }
                }
            });
            
            // Add payment table title
            doc.setFontSize(11);
            doc.text('TABEL ANGSURAN KREDIT', 105, doc.autoTable.previous.finalY + 15, { align: 'center' });
            
            // Prepare table data
            const headers = [
                'Bulan',
                'Pokok (Rp)',
                'Bunga (Rp)',
                'Total (Rp)',
                'Sisa (Rp)'
            ];
            
            const data = result.tabel.map(row => [
                row.bulan,
                formatNominal(row.angsuranPokok),
                formatNominal(row.angsuranBunga),
                formatNominal(row.totalAngsuran),
                formatNominal(row.sisaPokok)
            ]);
            
            // Add payment table
            doc.autoTable({
                startY: doc.autoTable.previous.finalY + 20,
                head: [headers],
                body: data,
                theme: 'grid',
                headStyles: {
                    fillColor: [79, 70, 229],
                    textColor: 255,
                    fontStyle: 'bold'
                },
                styles: {
                    cellPadding: 2,
                    fontSize: 7,
                    overflow: 'linebreak',
                    halign: 'right'
                },
                columnStyles: {
                    0: { halign: 'left', cellWidth: 15 } // Bulan rata kiri
                },
                alternateRowStyles: {
                    fillColor: [243, 244, 246]
                },
                margin: { horizontal: 5 }
            });
            
            // Add footer
            const pageCount = doc.internal.getNumberOfPages();
            for(let i = 1; i <= pageCount; i++) {
                doc.setPage(i);
                doc.setFontSize(7);
                doc.setTextColor(100, 100, 100);
                doc.text(`Halaman ${i} dari ${pageCount}`, 105, 285, { align: 'center' });
            }
            
            // Save PDF
            doc.save(`Laporan-Kredit-BPR-Penataran-${new Date().toISOString().slice(0,10)}.pdf`);
        }

        // Main calculation function
        function hitungSimulasi() {
            const pinjaman = parseFloat(parseNominal(document.getElementById('pinjaman').value));
            const lamaPinjaman = parseInt(document.getElementById('lamaPinjaman').value);
            const bungaTahunan = parseFloat(document.getElementById('bungaTahunan').value) / 100;
            const modePerhitungan = document.getElementById('modePerhitungan').value;

            if (!pinjaman || !lamaPinjaman || isNaN(bungaTahunan)) {
                alert('Harap isi semua field dengan benar');
                return;
            }

            let result;
            
            switch (modePerhitungan) {
                case 'flat':
                    result = hitungAngsuranFlat(pinjaman, bungaTahunan, lamaPinjaman);
                    break;
                case 'anuitas':
                    result = hitungAngsuranAnuitas(pinjaman, bungaTahunan, lamaPinjaman);
                    break;
                case 'efektif':
                    result = hitungAngsuranEfektif(pinjaman, bungaTahunan, lamaPinjaman);
                    break;
                default:
                    alert('Mode perhitungan tidak valid');
                    return;
            }

            // Update summary table
            document.getElementById('metodePerhitungan').textContent = result.ringkasan.metodePerhitungan;
            document.getElementById('jumlahPinjaman').textContent = `Rp ${formatNominal(result.ringkasan.pokok)}`;
            document.getElementById('lamaPinjamanSummary').textContent = `${result.ringkasan.tenor} Bulan`;
            document.getElementById('bungaTahunanSummary').textContent = `${Math.round(result.ringkasan.bungaTahunan)}%`;
            document.getElementById('angsuranBulanan').textContent = `Rp ${formatNominal(result.ringkasan.angsuranBulanan)}`;
            document.getElementById('totalBunga').textContent = `Rp ${formatNominal(result.ringkasan.totalBunga)}`;
            document.getElementById('totalPembayaran').textContent = `Rp ${formatNominal(result.ringkasan.totalPembayaran)}`;

            // Fill payment table
            const tabelBody = document.getElementById('tabelBody');
            tabelBody.innerHTML = '';
            
            result.tabel.forEach(row => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-indigo-50 even:bg-gray-50';
                
                tr.innerHTML = `
                    <td class="p-2 sm:p-3 border text-gray-700 text-sm">${row.bulan}</td>
                    <td class="p-2 sm:p-3 border text-gray-700 number-cell text-sm">${formatNominal(row.angsuranPokok)}</td>
                    <td class="p-2 sm:p-3 border text-gray-700 number-cell text-sm">${formatNominal(row.angsuranBunga)}</td>
                    <td class="p-2 sm:p-3 border text-gray-700 font-medium number-cell text-sm">${formatNominal(row.totalAngsuran)}</td>
                    <td class="p-2 sm:p-3 border text-gray-700 number-cell text-sm">${formatNominal(row.sisaPokok)}</td>
                `;
                
                tabelBody.appendChild(tr);
            });

            // Set up PDF download
            document.getElementById('downloadPdf').onclick = () => generatePDF(result);

            // Show results
            document.getElementById('resultsContainer').classList.remove('hidden');
            
            // Scroll to results
            document.getElementById('resultsContainer').scrollIntoView({ behavior: 'smooth' });
        }

        // Event listeners
        document.getElementById('hitungBtn').addEventListener('click', hitungSimulasi);

        // Format pinjaman input
        document.getElementById('pinjaman').addEventListener('input', function(e) {
            const value = parseNominal(e.target.value);
            if (!isNaN(value)) {
                e.target.value = formatNominal(value);
            }
        });

        // Handle keyboard enter
        document.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                hitungSimulasi();
            }
        });
    </script>
</body>
</html>
