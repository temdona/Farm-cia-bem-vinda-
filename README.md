# Farm-cia-bem-vinda-
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Farmácia BemVinda - Delivery</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: Arial, sans-serif; }
    body { background: #f9f9f9; color: #333; }
    header { background: #1e88e5; color: #fff; padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; }
    header h1 { font-size: 1.5rem; }
    nav a { color: #fff; margin-left: 15px; text-decoration: none; font-weight: bold; }
    nav a:hover { text-decoration: underline; }
    .banner { background: url('https://images.unsplash.com/photo-1580281658629-7a9a09b0660d?auto=format&fit=crop&w=1350&q=80') center/cover no-repeat; color: white; text-align: center; padding: 60px 20px; }
    .banner h2 { font-size: 2rem; margin-bottom: 10px; }
    .produtos { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; padding: 20px; max-width: 1200px; margin: auto; }
    .produto { background: white; border-radius: 8px; overflow: hidden; box-shadow: 0 2px 6px rgba(0,0,0,0.1); display: flex; flex-direction: column; }
    .produto img { width: 100%; height: 160px; object-fit: cover; }
    .produto h3 { padding: 10px; font-size: 1.1rem; }
    .produto p { padding: 0 10px; font-size: 0.9rem; flex-grow: 1; }
    .produto button { margin: 10px; padding: 10px; background: #1e88e5; border: none; color: white; font-weight: bold; border-radius: 5px; cursor: pointer; }
    .produto button:hover { background: #1565c0; }
    #carrinho { position: fixed; right: -320px; top: 0; width: 300px; height: 100%; background: white; box-shadow: -2px 0 8px rgba(0,0,0,0.2); transition: right 0.3s ease; display: flex; flex-direction: column; }
    #carrinho.aberto { right: 0; }
    #carrinho header { background: #1e88e5; color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center; }
    #lista-carrinho { flex-grow: 1; padding: 10px; overflow-y: auto; }
    #carrinho footer { padding: 10px; border-top: 1px solid #ddd; font-weight: bold; }
    footer { background: #1e88e5; color: white; text-align: center; padding: 15px; margin-top: 30px; }
    #botao-carrinho { position: fixed; top: 20px; right: 20px; background: #ff9800; color: white; border: none; padding: 10px 15px; border-radius: 50px; font-size: 1rem; cursor: pointer; box-shadow: 0 2px 6px rgba(0,0,0,0.2); }
    #botao-carrinho:hover { background: #e68900; }
    #finalizar-btn { background: #25d366; color: white; border: none; padding: 10px; width: 100%; font-weight: bold; cursor: pointer; margin-top: 10px; }
    #finalizar-btn:hover { background: #1ebe5d; }
  </style>
</head>
<body>

  <header>
    <h1>Farmácia BemVinda</h1>
    <nav>
      <a href="#">Início</a>
      <a href="#">Produtos</a>
      <a href="#">Contato</a>
    </nav>
  </header>

  <section class="banner">
    <h2>Delivery Rápido e Confiável</h2>
    <p>Receba seus medicamentos e produtos de saúde no conforto de casa.</p>
  </section>

  <main>
    <section class="produtos">
      <div class="produto">
        <img src="https://images.unsplash.com/photo-1580281658630-d78b68aa1f2a?auto=format&fit=crop&w=600&q=80" alt="Paracetamol">
        <h3>Paracetamol 500mg</h3>
        <p>Analgésico e antitérmico para alívio de dores e febre.</p>
        <button onclick="adicionarAoCarrinho('Paracetamol 500mg', 9.90)">Adicionar - R$ 9,90</button>
      </div>
      <div class="produto">
        <img src="https://images.unsplash.com/photo-1580281658534-4e0e7dba91b1?auto=format&fit=crop&w=600&q=80" alt="Vitamina C">
        <h3>Vitamina C</h3>
        <p>Fortalece o sistema imunológico e auxilia na prevenção de gripes.</p>
        <button onclick="adicionarAoCarrinho('Vitamina C', 14.50)">Adicionar - R$ 14,50</button>
      </div>
      <div class="produto">
        <img src="https://images.unsplash.com/photo-1580281658525-6eaf7f5d6e20?auto=format&fit=crop&w=600&q=80" alt="Álcool em Gel">
        <h3>Álcool em Gel 70%</h3>
        <p>Higienização das mãos e prevenção contra vírus e bactérias.</p>
        <button onclick="adicionarAoCarrinho('Álcool em Gel 70%', 7.50)">Adicionar - R$ 7,50</button>
      </div>
    </section>
  </main>

  <button id="botao-carrinho" onclick="toggleCarrinho()">🛒 Carrinho</button>

  <aside id="carrinho">
    <header>
      <span>Meu Carrinho</span>
      <button onclick="toggleCarrinho()" style="background:none;border:none;color:white;font-size:1.2rem;">✖</button>
    </header>
    <div id="lista-carrinho"></div>
    <footer>
      Total: R$ <span id="total-carrinho">0.00</span>
      <button id="finalizar-btn" onclick="finalizarPedido()">Finalizar Pedido</button>
    </footer>
  </aside>

  <footer>
    <p>&copy; 2025 Farmácia BemVinda - Todos os direitos reservados</p>
    <p>Contato: (11) 99999-9999 | Instagram: @farmaciabemvinda</p>
  </footer>

  <script>
    let carrinho = [];
    let total = 0;
    const numeroWhatsApp = "5511999999999"; // coloque o número com DDI 55 e DDD

    function adicionarAoCarrinho(produto, preco) {
      carrinho.push({ produto, preco });
      total += preco;
      atualizarCarrinho();
    }

    function atualizarCarrinho() {
      const lista = document.getElementById('lista-carrinho');
      lista.innerHTML = '';
      carrinho.forEach(item => {
        const div = document.createElement('div');
        div.textContent = `${item.produto} - R$ ${item.preco.toFixed(2)}`;
        lista.appendChild(div);
      });
      document.getElementById('total-carrinho').textContent = total.toFixed(2);
    }

    function toggleCarrinho() {
      document.getElementById('carrinho').classList.toggle('aberto');
    }

    function finalizarPedido() {
      if (carrinho.length === 0) {
        alert("Seu carrinho está vazio!");
        return;
      }
      let mensagem = "Olá! Gostaria de fazer um pedido:%0A";
      carrinho.forEach(item => {
        mensagem += `- ${item.produto} (R$ ${item.preco.toFixed(2)})%0A`;
      });
      mensagem += `%0ATotal: R$ ${total.toFixed(2)}`;
      window.open(`https://wa.me/${numeroWhatsApp}?text=${mensagem}`, "_blank");
    }
  </script>

</body>
</html>
