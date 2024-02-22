<script lang="ts">
  import { onMount } from "svelte";

  interface Contato {
    codigo: number;
    nome: string;
    telefone: string;
    endereco: string;
  }

  let contatos: Contato[] = [];
  onMount(() => {
    let dadosSalvos = localStorage.getItem("contatos");
    if (dadosSalvos) contatos = JSON.parse(dadosSalvos);
  });

  let novoContato: Contato = {
    codigo: 0,
    nome: "",
    telefone: "",
    endereco: "",
  };

  function editarContato(contato: Contato) {
    novoContato = { ...contato }; // Faz uma cópia do contato para o novoContato
  }

  function salvarContato() {
    if (novoContato.codigo === 0) {
      // Adicionando um novo contato
      novoContato.codigo = Math.floor(Math.random() * 10000) + 1;
      contatos = [...contatos, novoContato];
    } else {
      // Editando um contato existente
      contatos = contatos.map((c) =>
        c.codigo === novoContato.codigo ? { ...novoContato } : c
      );
    }
    localStorage.setItem("contatos", JSON.stringify(contatos)); // Salva a lista atualizada no localStorage
    novoContato = { codigo: 0, nome: "", telefone: "", endereco: "" }; // Reseta o formulário
  }

  function excluirContato(codigo: number) {
    contatos = contatos.filter((c) => c.codigo !== codigo);
    localStorage.setItem("contatos", JSON.stringify(contatos)); // Atualiza o localStorage
  }
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/water.css@2/out/water.css"
  />
</svelte:head>
<h1>Super Crud em Sveltekit</h1>
<table>
  <tr>
    <th>Código</th>
    <th>Nome</th>
    <th>Telefone</th>
    <th>Endereço</th>
    <th>Ações</th>
  </tr>
  {#each contatos as contato}
    <tr>
      <td>{contato.codigo}</td>
      <td>{contato.nome}</td>
      <td>{contato.telefone}</td>
      <td>{contato.endereco}</td>
      <td>
        <button on:click={() => editarContato(contato)}>Editar</button>
        <button on:click={() => excluirContato(contato.codigo)}>Excluir</button>
      </td>
    </tr>
  {/each}
</table>
<form>
  <input type="hidden" bind:value={novoContato.codigo} />
  <input type="text" placeholder="Nome" bind:value={novoContato.nome} />
  <input type="text" placeholder="Telefone" bind:value={novoContato.telefone} />
  <input type="text" placeholder="Endereço" bind:value={novoContato.endereco} />
  <button type="button" on:click={salvarContato}>Salvar</button>
</form>
