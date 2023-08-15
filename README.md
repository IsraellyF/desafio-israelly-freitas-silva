# desafio-israelly-freitas-silva
// Objeto que contém o cardápio com os valores dos itens
const cardapio = {
    cafe: { descricao: 'Café', valor: 3.00 },
    chantily: { descricao: 'Chantily (extra do Café)', valor: 1.50 },
    suco: { descricao: 'Suco Natural', valor: 6.20 },
    sanduiche: { descricao: 'Sanduíche', valor: 6.50 },
    queijo: { descricao: 'Queijo (extra do Sanduíche)', valor: 2.00 },
    salgado: { descricao: 'Salgado', valor: 7.25 },
    combo1: { descricao: '1 Suco e 1 Sanduíche', valor: 9.50 },
    combo2: { descricao: '1 Café e 1 Sanduíche', valor: 7.50 }
};

// Objeto que contém as formas de pagamento e seus ajustes de preço
const formasPagamento = {
    dinheiro: { desconto: 0.05, acrescimo: 0.0 },
    debito: { desconto: 0.0, acrescimo: 0.0 },
    credito: { desconto: 0.0, acrescimo: 0.03 }
};

// Função para calcular o valor total da compra
function calcularValorTotal(pedido, pagamento) {
    let total = 0;

    if (Object.keys(pedido).length === 0) {
        return 'Não há itens no carrinho de compra!';
    }

    for (const item in pedido) {
        if (!cardapio[item]) {
            return 'Item inválido!';
        }

        const valorItem = cardapio[item].valor;

        if (pedido[item] < 0) {
            return 'Quantidade inválida!';
        }

        total += valorItem * pedido[item];
    }

    if (!formasPagamento[pagamento]) {
        return 'Forma de pagamento inválida!';
    }

    const desconto = formasPagamento[pagamento].desconto;
    const acrescimo = formasPagamento[pagamento].acrescimo;

    total *= 1 - desconto;
    total *= 1 + acrescimo;

    return total.toFixed(2);
}

// Função para verificar a validade do pedido com itens extras
function validarItensExtras(pedido) {
    for (const item in pedido) {
        if (item.endsWith('_extra')) {
            const itemPrincipal = item.replace('_extra', '');
            if (!pedido[itemPrincipal] && !['combo1', 'combo2'].includes(itemPrincipal)) {
                return 'Item extra não pode ser pedido sem o principal.';
            }
        }
    }
    return null;
}

// Exemplo de uso
const pedido = {
    cafe: 1,
    chantily_extra: 1,
    suco: 2,
    combo1: 1
};

const pagamento = 'dinheiro';

const mensagemValidacao = validarItensExtras(pedido);
if (mensagemValidacao) {
    console.log(mensagemValidacao);
} else {
    const valorTotal = calcularValorTotal(pedido, pagamento);
    console.log(`Valor total da compra: R$ ${valorTotal}`);
}
