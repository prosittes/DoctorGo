<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>DoctorGo — Encontre seu médico</title>
  <!-- Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <meta name="description" content="DoctorGo — plataforma para encontrar médicos por especialidade e cidade, com avaliações e agendamento." />
  <style>
    /* fallback mínimo caso o Tailwind não carregue */
    .chip{display:inline-block;margin:4px 6px;padding:8px 14px;border-radius:999px;border:1px solid #e5e7eb;cursor:pointer}
    .chip.active{background:#111827;color:#fff;border-color:#111827}
    .scroll-x{overflow:auto;-webkit-overflow-scrolling:touch}
    .glass{backdrop-filter: blur(10px)}
  </style>
</head>
<body class="bg-gray-50 text-gray-800">

  <!-- Header -->
  <header class="bg-white/80 glass border-b sticky top-0 z-50">
    <div class="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <div class="w-9 h-9 rounded-2xl bg-indigo-600"></div>
        <span class="font-semibold text-lg">DoctorGo</span>
      </div>
      <nav class="flex items-center gap-3">
        <button id="btnLogin" class="px-4 py-2 rounded-xl border hover:bg-gray-100">Entrar</button>
        <button id="btnCad" class="px-4 py-2 rounded-xl bg-indigo-600 text-white hover:bg-indigo-700">Cadastrar</button>
      </nav>
    </div>
  </header>

  <!-- Hero -->
  <section class="bg-gradient-to-b from-indigo-50 to-transparent">
    <div class="max-w-6xl mx-auto px-4 py-10 md:py-16 grid md:grid-cols-2 gap-8 items-center">
      <div>
        <h1 class="text-3xl md:text-5xl font-bold leading-tight">Encontre especialistas de confiança perto de você</h1>
        <p class="mt-3 text-gray-600">Busque por especialidade, filtre por estado e cidade. Veja avaliações reais e agende em poucos toques.</p>
        <ul class="mt-4 space-y-2 text-sm text-gray-700">
          <li>• Limite de <strong>3 médicos por cidade/especialidade</strong> para evitar guerra de preços</li>
          <li>• <strong>Pix/Débito</strong>: taxa da plataforma descontada do médico</li>
          <li>• <strong>Crédito</strong>: pequena taxa repassada ao paciente</li>
        </ul>
      </div>
      <div class="bg-white border rounded-2xl p-4 md:p-5 shadow-sm">
        <h2 class="font-semibold text-lg mb-3">Buscar médico</h2>
        <div class="mb-3">
          <label class="block text-sm mb-1">Especialidade</label>
          <div id="chips" class="scroll-x whitespace-nowrap"></div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
          <div>
            <label class="block text-sm mb-1">Estado (UF)</label>
            <select id="uf" class="w-full border rounded-xl px-3 py-2">
              <option value="">Selecionar</option>
            </select>
          </div>
          <div>
            <label class="block text-sm mb-1">Cidade</label>
            <input id="city" class="w-full border rounded-xl px-3 py-2" placeholder="Ex.: São Paulo" />
          </div>
        </div>
        <button id="btnBuscar" class="mt-4 w-full rounded-xl bg-indigo-600 text-white py-2.5 hover:bg-indigo-700">Mostrar médicos</button>
      </div>
    </div>
  </section>

  <!-- Lista de médicos -->
  <section class="max-w-6xl mx-auto px-4 pb-16">
    <div id="resultsHeader" class="hidden mb-3 text-sm text-gray-600">Mostrando no máximo <strong>3</strong> profissionais</div>
    <div id="cards" class="grid sm:grid-cols-2 lg:grid-cols-3 gap-4"></div>
    <div id="noResults" class="hidden text-gray-500 mt-6">Nenhum médico encontrado nessa combinação.</div>
  </section>

  <!-- Agendar via WhatsApp (form paciente) -->
  <section class="bg-white border-t">
    <div class="max-w-6xl mx-auto px-4 py-10">
      <h3 class="text-xl font-semibold">Agendar via WhatsApp</h3>
      <p class="text-gray-600 text-sm mb-4">Preencha seus dados e enviaremos a mensagem direto no WhatsApp do DoctorGo.</p>
      <form id="waForm" class="grid md:grid-cols-3 gap-4">
        <input class="border rounded-xl px-3 py-2" id="f_nome" placeholder="Nome" required />
        <input class="border rounded-xl px-3 py-2" id="f_idade" placeholder="Idade" />
        <input class="border rounded-xl px-3 py-2" id="f_email" placeholder="E-mail" type="email" />
        <input class="border rounded-xl px-3 py-2" id="f_tel" placeholder="Telefone/WhatsApp" />
        <input class="border rounded-xl px-3 py-2" id="f_cidade" placeholder="Cidade" />
        <input class="border rounded-xl px-3 py-2" id="f_horario" placeholder="Melhor horário para contato" />
        <input class="border rounded-xl px-3 py-2 md:col-span-3" id="f_condicao" placeholder="Condição de saúde" />
        <textarea class="border rounded-xl px-3 py-2 md:col-span-3" id="f_sintomas" placeholder="Sintomas" rows="3"></textarea>
        <button class="rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white px-4 py-3 md:col-span-3" type="submit">Enviar no WhatsApp</button>
      </form>
      <p class="text-xs text-gray-500 mt-3">Ao enviar, você concorda com os <a href="#" id="lnkTermos1" class="underline">Termos</a> e a <a href="#" id="lnkPriv1" class="underline">Política de Privacidade</a>.</p>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-900 text-gray-300">
    <div class="max-w-6xl mx-auto px-4 py-8 flex flex-col md:flex-row items-start md:items-center justify-between gap-4">
      <div class="text-sm">© <span id="year"></span> DoctorGo. Todos os direitos reservados.</div>
      <div class="text-sm space-x-4">
        <a href="#" id="lnkTermos2" class="hover:underline">Termos de Uso</a>
        <a href="#" id="lnkPriv2" class="hover:underline">Política de Privacidade (LGPD)</a>
      </div>
    </div>
  </footer>

  <!-- Modal Login (Paciente) -->
  <div id="modalLogin" class="hidden fixed inset-0 bg-black/40 z-[60] flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl w-full max-w-md p-6 relative">
      <button class="absolute right-3 top-3 text-gray-500" onclick="toggle('modalLogin', false)">✕</button>
      <h3 class="text-xl font-semibold mb-1">Entrar</h3>
      <p class="text-sm text-gray-500 mb-4">Área do paciente</p>
      <form onsubmit="event.preventDefault(); alert('Login de exemplo. Integre ao Firebase Auth.');">
        <label class="block text-sm mb-1">Usuário (e-mail ou telefone)</label>
        <input class="w-full border rounded-xl px-3 py-2 mb-3" placeholder="usuario@exemplo.com" required />
        <label class="block text-sm mb-1">Senha</label>
        <input type="password" class="w-full border rounded-xl px-3 py-2 mb-3" placeholder="••••••••" required />
        <div class="flex items-center justify-between mb-4">
          <a href="#" class="text-sm text-indigo-700 hover:underline" onclick="alert('Recuperação de senha — integrar Firebase Auth.');">Esqueci minha senha</a>
        </div>
        <button class="w-full rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white py-2.5">Entrar</button>
      </form>
      <div class="text-sm text-gray-600 mt-4">
        Novo por aqui? <a href="#" class="text-indigo-700 hover:underline" onclick="toggle('modalLogin', false); toggle('modalCadastro', true);">Cadastrar novo usuário</a>
      </div>
    </div>
  </div>

  <!-- Modal Cadastro (com LGPD) -->
  <div id="modalCadastro" class="hidden fixed inset-0 bg-black/40 z-[60] flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl w-full max-w-md p-6 relative">
      <button class="absolute right-3 top-3 text-gray-500" onclick="toggle('modalCadastro', false)">✕</button>
      <h3 class="text-xl font-semibold mb-1">Cadastrar novo usuário</h3>
      <p class="text-sm text-gray-500 mb-4">Área do paciente</p>
      <form id="formCad" onsubmit="event.preventDefault(); alert('Cadastro de exemplo. Integre ao Firebase Auth/Firestore.');">
        <input class="w-full border rounded-xl px-3 py-2 mb-3" placeholder="Nome completo" required />
        <input type="email" class="w-full border rounded-xl px-3 py-2 mb-3" placeholder="E-mail" required />
        <input class="w-full border rounded-xl px-3 py-2 mb-3" placeholder="Telefone/WhatsApp" />
        <div class="grid grid-cols-2 gap-3 mb-3">
          <select id="ufCad" class="border rounded-xl px-3 py-2" required></select>
          <input class="border rounded-xl px-3 py-2" placeholder="Cidade" required />
        </div>
        <input type="password" class="w-full border rounded-xl px-3 py-2 mb-3" placeholder="Senha" required />
        <div class="space-y-2 text-sm">
          <label class="flex gap-2"><input id="chkTermos" type="checkbox" required /> Li e <a href="#" id="lnkTermos3" class="underline">aceito os Termos</a> e a <a href="#" id="lnkPriv3" class="underline">Política</a>.</label>
          <label class="flex gap-2"><input id="chkSaude" type="checkbox" required /> Consinto o tratamento de <strong>dados de saúde</strong> para fins de atendimento.</label>
          <label class="flex gap-2"><input id="chkMkt" type="checkbox" /> Quero receber novidades.</label>
        </div>
        <button id="btnCriar" class="mt-4 w-full rounded-xl bg-indigo-600 hover:bg-indigo-700 text-white py-2.5 disabled:opacity-50" disabled>Criar conta</button>
      </form>
    </div>
  </div>

  <!-- Modal Termos / Privacidade -->
  <div id="modalDocs" class="hidden fixed inset-0 bg-black/40 z-[70] p-4 overflow-y-auto">
    <div class="bg-white rounded-2xl max-w-3xl mx-auto p-6 relative">
      <button class="absolute right-3 top-3 text-gray-500" onclick="toggle('modalDocs', false)">✕</button>
      <h3 id="docTitle" class="text-xl font-semibold mb-2">Documento</h3>
      <article id="docBody" class="prose max-w-none">
        <!-- Conteúdo é injetado via JS (placeholders resumidos) -->
      </article>
    </div>
  </div>

  <script>
    // ========= Config =========
    const WHATSAPP_NUMBER = '55XXXXXXXXXXX'; // <-- Substitua pelo número oficial do DoctorGo (somente dígitos com DDI 55)
   const SPECIALTIES = ['Laser', 'Ginecologia', 'Odontologia', 'Fisioterapia', 'Dermatologia', 'Cardiologia'];
    const UFS = ["AC","AL","AP","AM","BA","CE","DF","ES","GO","MA","MT","MS","MG","PA","PB","PR","PE","PI","RJ","RN","RS","RO","RR","SC","SP","SE","TO"];

    // Médicos de exemplo (troque pelos reais ou traga via API/Firebase)
    const DOCTORS = [
      { uid:'d1', name:'Dra. Ana Laser', specialty:'Laser', city:'São Paulo', uf:'SP', ratingAvg:4.9, ratingCount:42, premium:true, priceHint:'A partir de R$ 220' },
      { uid:'d2', name:'Dr. João Laser', specialty:'Laser', city:'São Paulo', uf:'SP', ratingAvg:4.7, ratingCount:28, premium:false, priceHint:'A partir de R$ 200' },
      { uid:'d3', name:'Dra. Bia Laser', specialty:'Laser', city:'São Paulo', uf:'SP', ratingAvg:4.8, ratingCount:33, premium:false, priceHint:'A partir de R$ 210' },
      { uid:'d4', name:'Dra. Carla Gineco', specialty:'Ginecologia', city:'Rio de Janeiro', uf:'RJ', ratingAvg:4.6, ratingCount:19, premium:true, priceHint:'A partir de R$ 250' },
      { uid:'d5', name:'Dr. Lucas Odonto', specialty:'Odontologia', city:'Curitiba', uf:'PR', ratingAvg:4.5, ratingCount:12, premium:false, priceHint:'A partir de R$ 180' },
      { uid:'d6', name:'Dra. Mari Derma', specialty:'Dermatologia', city:'Belo Horizonte', uf:'MG', ratingAvg:4.8, ratingCount:51, premium:true, priceHint:'A partir de R$ 230' },
      { uid:'d7', name:'Dr. Paulo Físio', specialty:'Fisioterapia', city:'Salvador', uf:'BA', ratingAvg:4.4, ratingCount:22, premium:false, priceHint:'A partir de R$ 150' },
    ];{ uid:'d8', name:'Dra. Bruna Nascimento', specialty:'Ginecologia', city:'São José dos Campos', uf:'SP', ratingAvg:4.9, ratingCount:12, premium:true, priceHint:'A partir de R$ 250' },


    // ========= Helpers =========
    const $ = (s)=>document.querySelector(s);
    const el = (tag, cls)=>{ const e=document.createElement(tag); if(cls) e.className=cls; return e; }
    const toggle = (id, show)=>{ const m = document.getElementById(id); m.classList[show?'remove':'add']('hidden'); }

    function fillUF(selectId){
      const sel = document.getElementById(selectId);
      sel.innerHTML = '<option value="">Selecionar</option>';
      UFS.forEach(u=> {
        const o = document.createElement('option');
        o.value = u; o.textContent = u;
        sel.appendChild(o);
      });
    }

    function renderChips(){
      const c = document.getElementById('chips');
      c.innerHTML = '';
      SPECIALTIES.forEach((s,i)=>{
        const b = el('button','chip px-4 py-2 rounded-full border text-sm hover:bg-gray-100');
        b.textContent = s;
        b.dataset.value = s;
        b.onclick = ()=>{
          document.querySelectorAll('#chips .chip').forEach(x=>x.classList.remove('active'));
          b.classList.add('active');
        };
        c.appendChild(b);
      });
    }

    function filterDoctors(){
      const active = document.querySelector('#chips .active');
      const specialty = active ? active.dataset.value : '';
      const uf = $('#uf').value.trim();
      const city = $('#city').value.trim().toLowerCase();

      let arr = DOCTORS.filter(d => {
        const okSpec = specialty ? d.specialty === specialty : true;
        const okUF = uf ? d.uf === uf : true;
        const okCity = city ? d.city.toLowerCase().includes(city) : true;
        return okSpec && okUF && okCity;
      });

      // Ordena: Premium primeiro, depois ratingAvg desc
      arr.sort((a,b)=>{
        if(a.premium !== b.premium) return a.premium ? -1 : 1;
        return (b.ratingAvg||0) - (a.ratingAvg||0);
      });

      // Limita a 3
      arr = arr.slice(0,3);
      renderCards(arr);
      document.getElementById('resultsHeader').classList.toggle('hidden', arr.length===0);
      document.getElementById('noResults').classList.toggle('hidden', arr.length!==0);
    }

    function renderCards(list){
      const wrap = document.getElementById('cards');
      wrap.innerHTML = '';
      list.forEach(d=>{
        const card = el('div','bg-white border rounded-2xl p-4 shadow-sm flex flex-col');
        const top = el('div','flex items-start justify-between gap-2');
        const left = el('div','');
        left.innerHTML = `<div class="font-semibold">${d.name}</div>
          <div class="text-sm text-gray-600">${d.specialty} • ${d.city}/${d.uf}</div>`;
        const badge = d.premium ? '<span class="text-xs px-2 py-1 bg-amber-100 text-amber-800 rounded-full">Premium</span>' : '';
        top.appendChild(left);
        const right = el('div','flex items-center gap-2');
        right.innerHTML = badge;
        top.appendChild(right);

        const rating = el('div','mt-2 text-sm text-gray-700');
        rating.innerHTML = `⭐ ${d.ratingAvg?.toFixed(1) || '—'} <span class="text-gray-500">(${d.ratingCount||0})</span>`;
        const price = el('div','mt-1 text-sm text-gray-600');
        price.textContent = d.priceHint || '';

        const btns = el('div','mt-4 flex gap-2');
        const b1 = el('button','flex-1 rounded-xl border px-3 py-2 hover:bg-gray-50');
        b1.textContent = 'Ver detalhes';
        b1.onclick = ()=>alert('Exemplo — aqui pode abrir perfil do médico.');
        const b2 = el('button','flex-1 rounded-xl bg-emerald-600 text-white px-3 py-2 hover:bg-emerald-700');
        b2.textContent = 'Agendar';
        b2.onclick = ()=>preencherWhatsApp(d);
        btns.appendChild(b1); btns.appendChild(b2);

        card.appendChild(top);
        card.appendChild(rating);
        card.appendChild(price);
        card.appendChild(btns);
        wrap.appendChild(card);
      });
    }

    function preencherWhatsApp(doc){
  // Salva o WhatsApp do médico escolhido
  window.__selectedDocPhone = doc.phone || '';

  // Preenche automaticamente cidade/especialidade no form de WhatsApp
  $('#f_cidade').value = doc.city;
  const msg = `Olá, quero agendar consulta com ${doc.name} (${doc.specialty}) em ${doc.city}/${doc.uf}.`;
  $('#f_sintomas').value = msg;
  window.scrollTo({top: document.getElementById('waForm').offsetTop - 80, behavior:'smooth'});
}


    function sendWhatsApp(data){
  // Vai primeiro para o médico selecionado (ao clicar em "Agendar")
  const sel = (window.__selectedDocPhone && /^\d{11,13}$/.test(window.__selectedDocPhone))
    ? window.__selectedDocPhone
    : '';

  // Se ninguém foi selecionado, você pode (A) bloquear e avisar, ou (B) usar um número padrão
  // A) Bloquear e pedir para escolher um médico:
  if(!sel){
    alert('Escolha um médico clicando no botão "Agendar" para enviar direto para ele.');
    return;
  }

  const texto = `*DoctorGo – Nova solicitação*\n` +
    `Nome: ${data.nome}\nIdade: ${data.idade}\nCidade: ${data.cidade}\n` +
    `Condição: ${data.condicao}\nSintomas: ${data.sintomas}\n` +
    `Melhor horário: ${data.horario}\n` +
    `Contato: ${data.email} | ${data.tel}`;

  const url = `https://wa.me/${sel}?text=${encodeURIComponent(texto)}`;
  window.open(url, '_blank');
}

      }
      const texto = `*DoctorGo – Nova solicitação*\n` +
        `Nome: ${data.nome}\nIdade: ${data.idade}\nCidade: ${data.cidade}\n` +
        `Condição: ${data.condicao}\nSintomas: ${data.sintomas}\n` +
        `Melhor horário: ${data.horario}\n` +
        `Contato: ${data.email} | ${data.tel}`;
      const url = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(texto)}`;
      window.open(url, '_blank');
    }

    // Termos/Política (placeholders curtos)
    const DOCS = {
      termos: {
        title: 'Termos de Uso — DoctorGo',
        body: `
          <p>DoctorGo intermedia agendamentos e pagamentos entre pacientes e profissionais. Não prestamos serviços médicos.</p>
          <ul>
            <li>Pagamentos: Pix/Débito (taxa do médico) • Cartão (taxa repassada ao paciente).</li>
            <li>Avaliações visíveis após consultas concluídas.</li>
            <li>Respeitamos a legislação aplicável e podemos atualizar estes termos.</li>
          </ul>
        `
      },
      priv: {
        title: 'Política de Privacidade — LGPD',
        body: `
          <p>Coletamos dados pessoais para cadastro, agendamento e pagamento. Dados de saúde são sensíveis e tratados com consentimento.</p>
          <ul>
            <li>Bases legais: execução de contrato, consentimento, obrigações legais e legítimo interesse.</li>
            <li>Compartilhamento: profissionais escolhidos, gateways de pagamento e provedores de nuvem.</li>
            <li>Direitos LGPD: acesso, correção, exclusão e portabilidade.</li>
          </ul>
        `
      }
    };

    function openDoc(key){
      const d = DOCS[key];
      if(!d) return;
      document.getElementById('docTitle').textContent = d.title;
      document.getElementById('docBody').innerHTML = d.body;
      toggle('modalDocs', true);
    }

    // ========= Events =========
    document.getElementById('btnBuscar').onclick = filterDoctors;
    document.getElementById('btnLogin').onclick = ()=>toggle('modalLogin', true);
    document.getElementById('btnCad').onclick = ()=>toggle('modalCadastro', true);

    // Termos links
    ['lnkTermos1','lnkTermos2','lnkTermos3'].forEach(id=>{
      const a = document.getElementById(id); if(a) a.onclick = (e)=>{e.preventDefault(); openDoc('termos');};
    });
    ['lnkPriv1','lnkPriv2','lnkPriv3'].forEach(id=>{
      const a = document.getElementById(id); if(a) a.onclick = (e)=>{e.preventDefault(); openDoc('priv');};
    });

    // Cadastro: habilita botão apenas se checkboxes LGPD marcados
    const chkTermos = document.getElementById('chkTermos');
    const chkSaude = document.getElementById('chkSaude');
    const btnCriar = document.getElementById('btnCriar');
    [chkTermos, chkSaude].forEach(c => c && c.addEventListener('change', ()=>{
      btnCriar.disabled = !(chkTermos.checked && chkSaude.checked);
    }));
    document.getElementById('formCad').addEventListener('submit', ()=>{
      alert('Conta criada (exemplo). No app real, use Firebase Auth + Firestore e salve consentVersion/Date.');
      toggle('modalCadastro', false);
      toggle('modalLogin', true);
    });

    // WhatsApp form
    document.getElementById('waForm').addEventListener('submit', (e)=>{
      e.preventDefault();
      const data = {
        nome: $('#f_nome').value.trim(),
        idade: $('#f_idade').value.trim(),
        email: $('#f_email').value.trim(),
        tel: $('#f_tel').value.trim(),
        cidade: $('#f_cidade').value.trim(),
        horario: $('#f_horario').value.trim(),
        condicao: $('#f_condicao').value.trim(),
        sintomas: $('#f_sintomas').value.trim()
      };
      if(!data.nome){ alert('Informe seu nome.'); return; }
      sendWhatsApp(data);
    });

    // Inicialização
    document.getElementById('year').textContent = new Date().getFullYear();
    renderChips();
    fillUF('uf'); fillUF('ufCad');
  </script>
</body>
</html>
