<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>BarberSys Pro - Gestão Total</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        input, select, button { font-size: 16px !important; }
        ::-webkit-scrollbar { display: none; }
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.8); z-index: 50; align-items: center; justify-content: center; }
        .modal.active { display: flex; }
    </style>
</head>
<body class="bg-zinc-900 text-zinc-100 pb-20">

    <div class="max-w-md mx-auto p-4">
        <header class="text-center py-6 border-b border-zinc-800 mb-6">
            <h1 id="viewNomeBarbearia" class="text-4xl font-black text-amber-500 uppercase italic">BarberSys</h1>
            <p id="viewBarbeiro" class="text-zinc-400 text-[10px] italic">SISTEMA DE GESTÃO PROFISSIONAL</p>
        </header>

        <section id="areaCliente" class="bg-zinc-800 p-6 rounded-[2.5rem] border border-zinc-700 shadow-2xl mb-4">
            <h2 class="text-[10px] font-black uppercase text-zinc-500 mb-4 text-center italic">Novo Agendamento</h2>
            <div class="space-y-4">
                <input type="text" id="cliente" placeholder="Seu Nome" class="w-full bg-zinc-900 border border-zinc-700 rounded-2xl p-5 outline-none">
                <input type="tel" id="telefone" placeholder="WhatsApp (DDD + Número)" class="w-full bg-zinc-900 border border-zinc-700 rounded-2xl p-5 outline-none">
                
                <select id="servico" class="w-full bg-zinc-900 border border-zinc-700 rounded-2xl p-5 font-bold">
                    <option value="Corte" id="optCorte">Corte</option>
                    <option value="Barba" id="optBarba">Barba</option>
                    <option value="Combo" id="optCombo">Combo</option>
                </select>

                <input type="date" id="dataAgendamento" onchange="window.gerarSlots()" class="w-full bg-zinc-900 border border-zinc-700 rounded-2xl p-5">

                <div id="boxHorarios" class="hidden">
                    <div id="gridHorarios" class="grid grid-cols-3 gap-2"></div>
                </div>

                <input type="hidden" id="horaSelecionada">
                <button id="btnConfirmar" onclick="window.salvarAgendamento()" class="w-full bg-zinc-700 text-zinc-500 font-black py-6 rounded-2xl uppercase italic">
                    Selecione uma data
                </button>
            </div>
        </section>

        <div class="text-center mb-10">
            <button onclick="document.getElementById('modalCancelamento').classList.add('active')" class="text-zinc-500 text-[10px] font-bold uppercase underline">Meus Agendamentos / Cancelar</button>
        </div>

        <div class="text-center mb-10">
            <button id="btnToggleAdmin" onclick="document.getElementById('formLogin').classList.toggle('hidden')" class="text-zinc-600 text-[10px] font-bold uppercase">Painel Administrativo</button>
            <div id="formLogin" class="hidden mt-6 bg-zinc-800 p-6 rounded-[2rem] border border-zinc-700 text-left">
                <input type="email" id="email" placeholder="E-mail" class="w-full bg-zinc-900 p-4 rounded-2xl mb-2 border border-zinc-700">
                <input type="password" id="senha" placeholder="Senha" class="w-full bg-zinc-900 p-4 rounded-2xl mb-4 border border-zinc-700">
                <button onclick="window.loginAdmin()" class="w-full bg-amber-500 text-black py-4 rounded-2xl font-black uppercase">Entrar</button>
            </div>
        </div>

        <div id="painelAdmin" class="hidden space-y-6">
            <div class="bg-blue-600/10 p-6 rounded-[2rem] border border-blue-600/50">
                <h3 class="text-blue-400 font-black text-[10px] uppercase mb-4 text-center italic">Configurações Pro</h3>
                <div class="space-y-3">
                    <input type="text" id="setEmpresa" placeholder="Nome da Barbearia" class="w-full bg-zinc-900 p-3 rounded-xl border border-zinc-700 text-xs">
                    
                    <div class="grid grid-cols-2 gap-2">
                        <div class="bg-zinc-900 p-2 rounded-xl border border-zinc-700">
                            <label class="text-[8px] text-zinc-500 uppercase block">Corte Preço/Custo</label>
                            <div class="flex gap-1">
                                <input type="number" id="pCorte" placeholder="Preço" class="w-1/2 bg-transparent text-green-500 font-bold outline-none">
                                <input type="number" id="cCorte" placeholder="Custo" class="w-1/2 bg-transparent text-red-500 outline-none">
                            </div>
                        </div>
                        <div class="bg-zinc-900 p-2 rounded-xl border border-zinc-700">
                            <label class="text-[8px] text-zinc-500 uppercase block">Barba Preço/Custo</label>
                            <div class="flex gap-1">
                                <input type="number" id="pBarba" placeholder="Preço" class="w-1/2 bg-transparent text-green-500 font-bold outline-none">
                                <input type="number" id="cBarba" placeholder="Custo" class="w-1/2 bg-transparent text-red-500 outline-none">
                            </div>
                        </div>
                    </div>
                    <input type="text" id="chaveMestra" placeholder="Digite a Chave JMV para salvar" class="w-full bg-zinc-900 p-3 rounded-xl border border-zinc-700 text-xs text-center border-amber-500/50">
                    <button onclick="window.atualizarConfig()" class="w-full bg-blue-600 py-4 rounded-2xl font-black text-[10px] uppercase">Salvar Alterações</button>
                </div>
            </div>

            <div class="bg-zinc-800 p-6 rounded-[2rem] border border-zinc-700 grid grid-cols-2 gap-4">
                <div class="text-center">
                    <p class="text-zinc-500 text-[8px] uppercase">Faturamento</p>
                    <span id="faturamentoGeral" class="text-green-500 font-black text-xl">R$ 0,00</span>
                </div>
                <div class="text-center border-l border-zinc-700">
                    <p class="text-zinc-500 text-[8px] uppercase">Lucro Líquido</p>
                    <span id="lucroGeral" class="text-amber-500 font-black text-xl">R$ 0,00</span>
                </div>
            </div>

            <div class="bg-zinc-800 p-6 rounded-[2.5rem] border border-zinc-700">
                <h3 class="text-amber-500 font-black text-[10px] uppercase mb-6 text-center italic">Agenda de Clientes</h3>
                <div id="listaGeral" class="space-y-3"></div>
                <button onclick="window.logout()" class="w-full mt-10 text-red-500 text-[10px] font-bold uppercase underline">Sair do Sistema</button>
            </div>
        </div>
    </div>

    <div id="modalCancelamento" class="modal">
        <div class="bg-zinc-800 p-8 rounded-[2rem] w-80 text-center border border-zinc-700">
            <h3 class="font-black uppercase text-amber-500 mb-4">Meus Agendamentos</h3>
            <input type="tel" id="telBusca" placeholder="Seu WhatsApp" class="w-full bg-zinc-900 p-4 rounded-xl mb-4 border border-zinc-700">
            <button onclick="window.buscarAgendamentosCliente()" class="w-full bg-amber-500 text-black py-3 rounded-xl font-bold mb-4 uppercase text-xs">Ver Meus Horários</button>
            <div id="resultadoBusca" class="space-y-2 mb-4 max-h-40 overflow-y-auto"></div>
            <button onclick="this.parentElement.parentElement.classList.remove('active')" class="text-zinc-500 text-[10px] uppercase">Fechar</button>
        </div>
    </div>

    <div id="modalReagendar" class="modal">
        <div class="bg-zinc-800 p-8 rounded-[2rem] w-80 text-center border border-zinc-700">
            <h3 class="font-black uppercase text-blue-500 mb-4">Reagendar</h3>
            <input type="hidden" id="reagendarId">
            <input type="date" id="novaData" class="w-full bg-zinc-900 p-4 rounded-xl mb-2 border border-zinc-700">
            <input type="time" id="novaHora" class="w-full bg-zinc-900 p-4 rounded-xl mb-4 border border-zinc-700">
            <button onclick="window.confirmarReagendamento()" class="w-full bg-blue-600 py-3 rounded-xl font-bold uppercase text-xs">Confirmar Mudança</button>
            <button onclick="this.parentElement.parentElement.classList.remove('active')" class="text-zinc-500 text-[10px] uppercase mt-4 block mx-auto">Voltar</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, orderBy, deleteDoc, doc, setDoc, getDocs, where, updateDoc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
        import { getAuth, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

        const firebaseConfig = {
            apiKey: "AIzaSyBUwkrtdnDb7_j0Yhj-z-srBb1nsmXIOrM",
            authDomain: "barbearia-vasconcelos.firebaseapp.com",
            projectId: "barbearia-vasconcelos",
            storageBucket: "barbearia-vasconcelos.firebasestorage.app",
            messagingSenderId: "642432616778",
            appId: "1:642432616778:web:f803e649f90cfeb32cd9b3"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        let configGlobal = { precos: {Corte:0, Barba:0, Combo:0}, custos: {Corte:0, Barba:0, Combo:0}, nomeBarbearia: "BarberSys" };

        window.loginAdmin = () => signInWithEmailAndPassword(auth, document.getElementById('email').value, document.getElementById('senha').value).catch(() => alert("Acesso Negado!"));
        window.logout = () => signOut(auth).then(() => location.reload());

        onSnapshot(doc(db, "configuracoes", "admin"), (s) => {
            if(s.exists()){
                const data = s.data();
                configGlobal = data;
                document.getElementById('viewNomeBarbearia').innerText = data.nomeBarbearia;
                document.getElementById('optCorte').innerText = `Corte - R$ ${data.precos.Corte}`;
                document.getElementById('optBarba').innerText = `Barba - R$ ${data.precos.Barba}`;
                document.getElementById('optCombo').innerText = `Combo - R$ ${data.precos.Combo}`;
                
                if(document.getElementById('pCorte')){
                    document.getElementById('setEmpresa').value = data.nomeBarbearia;
                    document.getElementById('pCorte').value = data.precos.Corte;
                    document.getElementById('cCorte').value = data.custos.Corte;
                    document.getElementById('pBarba').value = data.precos.Barba;
                    document.getElementById('cBarba').value = data.custos.Barba;
                }
            }
        });

        window.atualizarConfig = async () => {
            const chave = document.getElementById('chaveMestra').value;
            if(chave !== "JMV") return alert("Chave JMV Inválida!");
            
            const pC = Number(document.getElementById('pCorte').value);
            const cC = Number(document.getElementById('cCorte').value);
            const pB = Number(document.getElementById('pBarba').value);
            const cB = Number(document.getElementById('cBarba').value);

            await setDoc(doc(db, "configuracoes", "admin"), {
                nomeBarbearia: document.getElementById('setEmpresa').value,
                precos: { Corte: pC, Barba: pB, Combo: (pC + pB) },
                custos: { Corte: cC, Barba: cB, Combo: (cC + cB) }
            }, {merge:true});
            alert("Salvo com sucesso!");
        };

        window.gerarSlots = async () => {
            const data = document.getElementById('dataAgendamento').value;
            const grid = document.getElementById('gridHorarios');
            document.getElementById('boxHorarios').classList.remove('hidden');
            const snap = await getDocs(query(collection(db, "agendamentos"), where("dataSimplificada", "==", data)));
            const ocupados = snap.docs.map(d => d.data().hora);
            const slots = ["08:00", "09:00", "10:00", "11:00", "13:00", "14:00", "15:00", "16:00", "17:00", "18:00", "19:00"];
            grid.innerHTML = "";
            slots.forEach(h => {
                const b = document.createElement('button');
                b.innerText = h;
                if(ocupados.includes(h)){
                    b.className = "bg-red-900/20 text-red-700 p-4 rounded-xl border border-red-900/30 cursor-not-allowed";
                } else {
                    b.className = "bg-zinc-900 p-4 rounded-xl border border-zinc-700 text-xs";
                    b.onclick = () => { 
                        document.getElementById('horaSelecionada').value = h;
                        document.getElementById('btnConfirmar').className = "w-full bg-green-600 text-white font-black py-6 rounded-2xl";
                        document.getElementById('btnConfirmar').innerText = "CONFIRMAR " + h;
                    };
                }
                grid.appendChild(b);
            });
        };

        window.salvarAgendamento = async () => {
            const n = document.getElementById('cliente').value;
            const t = document.getElementById('telefone').value.replace(/\D/g,'');
            const s = document.getElementById('servico').value;
            const d = document.getElementById('dataAgendamento').value;
            const h = document.getElementById('horaSelecionada').value;
            if(!n || !t || !h || !d) return alert("Preencha todos os campos!");

            await addDoc(collection(db, "agendamentos"), {
                cliente: n, telefone: t, servico: s, dataSimplificada: d, hora: h,
                preco: configGlobal.precos[s] || 0, custo: configGlobal.custos[s] || 0, criadoEm: new Date().toISOString()
            });
            alert("Agendado com sucesso!"); location.reload();
        };

        window.buscarAgendamentosCliente = async () => {
            const tel = document.getElementById('telBusca').value.replace(/\D/g,'');
            const res = document.getElementById('resultadoBusca');
            res.innerHTML = "Buscando...";
            const snap = await getDocs(query(collection(db, "agendamentos"), where("telefone", "==", tel)));
            res.innerHTML = snap.empty ? "Nenhum horário encontrado." : "";
            snap.forEach(d => {
                const ag = d.data();
                const item = document.createElement('div');
                item.className = "bg-zinc-900 p-3 rounded-lg text-[10px] flex justify-between items-center border border-zinc-700";
                item.innerHTML = `<span>${ag.dataSimplificada} - ${ag.hora}</span> <button onclick="window.cancelarProprio('${d.id}')" class="text-red-500 font-bold">CANCELAR</button>`;
                res.appendChild(item);
            });
        };
        window.cancelarProprio = async (id) => { if(confirm("Deseja cancelar?")) { await deleteDoc(doc(db, "agendamentos", id)); alert("Cancelado!"); location.reload(); }};

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('painelAdmin').classList.remove('hidden');
                document.getElementById('areaCliente').classList.add('hidden');
                carregarAgendaAdmin();
            }
        });

        function carregarAgendaAdmin() {
            onSnapshot(query(collection(db, "agendamentos"), orderBy("dataSimplificada", "desc")), (snap) => {
                const lista = document.getElementById('listaGeral');
                lista.innerHTML = "";
                let fat = 0, luc = 0;
                snap.forEach(s => {
                    const ag = s.data();
                    fat += (ag.preco || 0); luc += ((ag.preco || 0) - (ag.custo || 0));
                    const card = document.createElement('div');
                    card.className = "bg-zinc-900 p-4 rounded-2xl border border-zinc-800 flex justify-between items-center";
                    card.innerHTML = `<div><p class="text-amber-500 font-black text-[10px]">${ag.dataSimplificada} - ${ag.hora}</p><p class="font-bold text-sm uppercase">${ag.cliente}</p></div>
                        <div class="flex gap-1">
                            <button onclick="window.reagendar('${s.id}')" class="bg-blue-500/10 text-blue-500 p-3 rounded-xl text-[10px]">✎</button>
                            <button onclick="window.zap('${ag.telefone}', '${ag.cliente}', '${ag.dataSimplificada}', '${ag.hora}')" class="bg-green-500/10 text-green-500 p-3 rounded-xl text-[10px]">ZAP</button>
                            <button onclick="window.cancelarProprio('${s.id}')" class="bg-red-500/10 text-red-500 p-3 rounded-xl text-[10px]">X</button>
                        </div>`;
                    lista.appendChild(card);
                });
                document.getElementById('faturamentoGeral').innerText = `R$ ${fat.toFixed(2)}`;
                document.getElementById('lucroGeral').innerText = `R$ ${luc.toFixed(2)}`;
            });
        }

        window.zap = (tel, nome, data, hora) => {
            const msg = encodeURIComponent(`Olá ${nome}! Confirmamos seu agendamento na ${configGlobal.nomeBarbearia} para o dia ${data} às ${hora}.`);
            window.open(`https://wa.me/55${tel}?text=${msg}`);
        };

        window.reagendar = (id) => {
            document.getElementById('reagendarId').value = id;
            document.getElementById('modalReagendar').classList.add('active');
        };

        window.confirmarReagendamento = async () => {
            const id = document.getElementById('reagendarId').value;
            const d = document.getElementById('novaData').value;
            const h = document.getElementById('novaHora').value;
            if(!d || !h) return alert("Escolha data e hora!");
            await updateDoc(doc(db, "agendamentos", id), { dataSimplificada: d, hora: h });
            alert("Reagendado!"); location.reload();
        };
    </script>
</body>
</html>
