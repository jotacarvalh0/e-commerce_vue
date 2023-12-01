<template>
    <div class="Shop">

      <div class="produtos-carrinho pt-64 pb-32">
      <div class="container">
          <h3>Carrinho de Compras</h3>
          <div class="lista-produtos-carrinho mt-32">

              <div class="produto-carrinho display-flex justify-content-space-between align-items-center gap-16 pb-24" v-for="produto in produtosCarr" :key="produto.id">
                  <div class="detalhes display-flex align-items-center gap-16">
                      <figure>
                          <img :src="produto.image">
                      </figure>
                      <article>
                          <p class="nome-produto">{{ produto.title }}</p>
                          <p class="categoria">{{ produto.categoria }}</p>
                          <p class="preco">R$ {{ produto.preco.toFixed(2).replace('.',',') }}</p>
                      </article>
                  </div>
                  <button @click="removerDoCarrinho(produto.id)">
                    
                      <svg width="24px" height="24px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                          <path d="M10 10V16M14 10V16M18 6V18C18 19.1046 17.1046 20 16 20H8C6.89543 20 6 19.1046 6 18V6M4 6H20M15 6V5C15 3.89543 14.1046 3 13 3H11C9.89543 3 9 3.89543 9 5V6" stroke="#ffffff" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                      </svg>
                      Remover do carrinho</button>
              </div>

              <p class="total">Total: R$ {{ calcularTotalCarrinho().toFixed(2).replace('.', ',') }}</p>
          </div>
      </div>
    </div>

  </div>
</template>

<script>
    export default {
        name: 'shopView',
        data(){
            return {
                produtosCarr:[]
            }
        },
        created() {
            this.requisicaoCarrinho()
        },
        methods: {
            requisicaoCarrinho(){
                fetch('http://localhost:3000/carrinho')
                .then( Response => Response.json() )
                .then( data => {
                    this.produtosCarr = data
                }) 
                .catch( error => {
                    console.log("Erro na requisicão", error)
                } )
            },
            calcularTotalCarrinho() {
              if (Array.isArray(this.produtosCarr) && this.produtosCarr.length > 0) {
                return this.produtosCarr.reduce((total, produto) => total + produto.preco, 0);
              } else {
                return 0; // ou qualquer valor padrão que você preferir
              }
            },
            async removerDoCarrinho(produtoId) {
              try {
                // Remover o produto do carrinho
                await fetch(`http://localhost:3000/carrinho/${produtoId}`, {
                  method: 'DELETE',
                });
            
                // Aguardar um curto período para garantir que a remoção seja processada
                await new Promise(resolve => setTimeout(resolve, 100));
            
                // Atualizar a lista de produtos no carrinho após a remoção
                await this.requisicaoCarrinho();
            
                // Calcular o total do carrinho novamente
                this.calcularTotalCarrinho();
              } catch (error) {
                console.error('Erro ao remover produto do carrinho', error);
              }
            },
        }
    }
</script>

<style>
.detalhes figure img {
    width: 250px;
    height: 250px;
}
</style>