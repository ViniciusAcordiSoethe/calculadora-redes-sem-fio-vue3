<script>
function load(type) {
    reset();
    document.getElementById(type).style = 'display: block';
}

function reset() {
    var calculatorBoxes = document.querySelectorAll(".calculator-box");

    for (var i = 0; i < calculatorBoxes.length; i++) {
        calculatorBoxes[i].style.display = "none";
    }
}

// Função para calcular a Capacidade Máxima de Canal (Shannon)
function shannon(larguraBandaHz, sinalRuidoDb) {
    return larguraBandaHz * Math.log2(1 + Math.pow(10, sinalRuidoDb / 10));
}

// Função para determinar a Taxa de Nyquist
function nyquist(larguraBandaHz, modulacaoMultinivel) {
    return 2 * larguraBandaHz * modulacaoMultinivel;
}

// Função para realizar a conversão de mW para dBm
function mwToDbm(potenciaMW) {
    return 10 * Math.log10(potenciaMW);
}

// Função para efetuar a conversão de dBm para mW
function dbmToMw(potenciaDBm) {
    return Math.pow(10, potenciaDBm / 10);
}

// Função para calcular a EIRP (Effective Isotropic Radiated Power)
function eirp(potenciaTransmissaoDBm, ganhoAntenaDBi, perdasCaboDB) {
    return potenciaTransmissaoDBm + ganhoAntenaDBi - perdasCaboDB;
}

// Função para determinar o FSLP (Free Space Loss Path)
function fslp(distanciaKm, frequenciaMHz) {
    return 32.4 + 20 * Math.log10(distanciaKm) + 20 * Math.log10(frequenciaMHz);
}

// Função para calcular o RSL (Received Signal Level)
function rsl(potenciaTransmissaoDBm, ganhoAntenaTXDBi, perdasCaboTXDB, ganhoAntenaRXDBi, perdasCaboRXDB, distanciaKm, frequenciaMHz) {
    const freeSpaceLossPath = calcularFSLP(distanciaKm, frequenciaMHz);
    return potenciaTransmissaoDBm + ganhoAntenaTXDBi - perdasCaboTXDB - freeSpaceLossPath + ganhoAntenaRXDBi - perdasCaboRXDB;
}

// Função para determinar o Raio da zona de Fresnel
function fresnel(DAO, DBO, distanciaKm, frequenciaMHz) {
    return 550 * Math.sqrt((DAO * DBO) / (distanciaKm * frequenciaMHz));
}
</script>
<template>
<div id="background-top">
        <div id="top-info">
            <img src="/logo.png" alt="Logo" id="logo">
            <div id="text-box">
                <h2 class="text-white">WiCalc IFC - Calculadora de redes sem fio</h2>
                <h4 class="text-white">O objetivo deste trabalho é criar uma ferramenta web interativa que permita o planejamento e análise de enlaces outdoor em redes sem fio.</h4>
            </div>
        </div>
    </div>
    <main>
        <div id="operations-container">
            <h2 id="main-title" class="text-gray">Selecione uma operação:</h2>
            <div id="operations-box">
                <div class="operations-line">
                    <div class="operation" onclick="load('shannon')">
                        <i class="fa-solid fa-arrow-up-wide-short operation-icon text-white"></i>
                        <span class="operation-name text-white">Shannon</span>
                    </div>
                    <div class="operation" onclick="load('nyquist')">
                        <i class="fa-solid fa-arrow-down-short-wide operation-icon text-white"></i>
                        <span class="operation-name text-white">Nyquist</span>
                    </div>
                    <div class="operation" onclick="load('mwdbm')">
                        <i class="fa-solid fa-right-left operation-icon text-white"></i>
                        <span class="operation-name text-white">mW para dBm</span>
                    </div>
                    <div class="operation" onclick="load('dbmmw')">
                        <i class="fa-solid fa-right-left fa-flip-horizontal operation-icon text-white"></i>
                        <span class="operation-name text-white">dBm para mW</span>
                    </div>
                </div>

                <div class="operations-line">
                    <div class="operation" onclick="load('eirp')">
                        <i class="fa-solid fa-satellite-dish operation-icon text-white"></i>
                        <span class="operation-name text-white">EIRP</span>
                    </div>
                    <div class="operation" onclick="load('fslp')">
                        <i class="fa-solid fa-signal fa-flip-horizontal operation-icon text-white"></i>
                        <span class="operation-name text-white">FSLP</span>
                    </div>
                    <div class="operation" onclick="load('rsl')">
                        <i class="fa-solid fa-satellite-dish fa-flip-horizontal operation-icon text-white"></i>
                        <span class="operation-name text-white">RSL</span>
                    </div>
                    <div class="operation" onclick="load('fresnel')">
                        <i class="fa-solid fa-house-signal operation-icon text-white"></i>
                        <span class="operation-name text-white">Fresnel Zone</span>
                    </div>
                </div>
            </div>
        </div>
        <div id="calculator-container">
            <div class="calculator-box" id="shannon" style="display: block;">
                <h3 class="calculator-title text-white">Shannon - Capacidade Máxima de Canal:</h3>
                <h5 class="calculator-description text-white">Calcular a capacidade máxima de transmissão de dados em um canal de comunicação</h5>
            </div>
            <div class="calculator-box" id="nyquist">
                <h3 class="calculator-title text-white">Nyquist - Taxa de Nyquist:</h3>
                <h5 class="calculator-description text-white">Determinar a taxa de amostragem mínima necessária para recuperar um sinal adequadamente</h5>
            </div>
            <div class="calculator-box" id="mwdbm">
                <h3 class="calculator-title text-white">Conversão de mW para dBm:</h3>
                <h5 class="calculator-description text-white">Realizar a conversão de potência de miliwatts (mW) para decibéis-milliwatts (dBm)</h5>
            </div>
            <div class="calculator-box" id="dbmmw">
                <h3 class="calculator-title text-white">Conversão de dBm para mW:</h3>
                <h5 class="calculator-description text-white">Efetuar a conversão de decibéis-milliwatts (dBm) para miliwatts (mW)</h5>
            </div>
            <div class="calculator-box" id="eirp">
                <h3 class="calculator-title text-white">EIRP (Effective Isotropic Radiated Power):</h3>
                <h5 class="calculator-description text-white">Calcular a potência efetiva irradiada isotropicamente por uma antena</h5>
            </div>
            <div class="calculator-box" id="fslp">
                <h3 class="calculator-title text-white">FSLP (Free Space Loss Path):</h3>
                <h5 class="calculator-description text-white">Determinar a perda de potência em uma transmissão sem fio em um espaço livre de obstáculos</h5>
            </div>
            <div class="calculator-box" id="rsl">
                <h3 class="calculator-title text-white">RSL (Received Signal Level):</h3>
                <h5 class="calculator-description text-white">Calcular o nível de sinal recebido em um enlace de comunicação sem fio</h5>
            </div>
            <div class="calculator-box" id="fresnel">
                <h3 class="calculator-title text-white">Fresnel Zone:</h3>
                <h5 class="calculator-description text-white">Determinar o raio da zona de Fresnel, uma área crucial que deve ser mantida livre de obstruções para garantir uma comunicação confiável</h5>
            </div>
        </div>
    </main>

    <footer>
        <div id="footer-icons">
            <img src="media/logo-gray.png" alt="Logo" class="footer-logo">
            <img src="media/ifc-gray.png" alt="Logo" class="footer-logo">
        </div>
        <div class="footer-info text-gray"><b>Desenvolvido por: Gustavo J. Baierski</b></div>
        <div class="footer-info text-gray">Redes sem fio</div>
        <div class="footer-info text-gray">2023</div>
    </footer>
</template>

<style scoped></style>
