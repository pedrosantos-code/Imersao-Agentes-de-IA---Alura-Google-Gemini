# Imersão-Agentes-de-IA---Alura + Google-Gemini

## ⚠️ Aviso Importante: Chave de API do Google Gemini

<p align="justify">Para executar este projeto, você precisará de uma chave de API do Google AI Studio.</p>

*   <strong>Obtenha sua chave aqui:</strong> <p align="justify">Visite o [Google AI Studio](https://aistudio.google.com/app/apikey) para gerar sua chave.</p>

*   <p align="justify"><strong>Segurança:</strong> É <strong>altamente recomendado</strong> que você utilize métodos seguros para armazenar e acessar sua chave, como os <strong>Google Colab Secrets</strong> (o ícone de chave 🔑 na barra lateral esquerda do Colab) ou variáveis de ambiente, <strong>evitando</strong> inseri-la diretamente no código-fonte, especialmente se for compartilhar o notebook ou usá-lo em produção. O notebook inclui uma opção para inserir a chave diretamente para testes rápidos, mas use com cautela.</p>

## 📄 Utilizando seus Próprios Documentos PDF

<p align="justify">Este notebook foi modificado para permitir que você carregue seus próprios arquivos PDF diretamente no ambiente do Google Colab no momento da execução.</p>

*   <p align="justify"><strong>Quando chegar na seção de carregamento de documentos</strong>, o notebook solicitará o upload dos arquivos.</p>
*   <p align="justify"><strong>Clique no botão de upload</strong> e selecione os arquivos PDF que contêm as informações que você deseja que o modelo utilize (por exemplo, políticas internas da sua empresa).</p>
*   <p align="justify"><strong>Os documentos de exemplo</strong> que estavam no Google Drive foram removidos do fluxo principal para facilitar o uso.</p>


## Aula 1

<p align="justify">
Primeiro, instalei umas ferramentas (<strong>langchain, langchain-google-genai, google-generativeai</strong>) pra eu poder conversar com o Google Gemini. Aí, configurei minha <strong>chave</strong> pra ter acesso (aquela que você pegou no Google IA Studio e colocou aqui no Colab). Depois, criei uma instância do modelo <strong>gemini-2.5-flash</strong>, configurei ele pra ser bem direto nas respostas e fiz um teste pra ver se a gente tava se entendendo. O mais legal foi quando me ensinaram a classificar as mensagens dos usuários: eu aprendi a analisar se a pergunta era simples de responder (<strong>'AUTO_RESOLVER'</strong>), se faltava informação (<strong>'PEDIR_INFO'</strong>) ou se era algo mais complexo que precisava abrir um chamado (<strong>'ABRIR_CHAMADO'</strong>), e ainda a definir a urgência. Pra garantir que eu classificasse tudo certinho, me deram um <strong>"molde" de resposta (TriagemOut)</strong> pra eu sempre seguir. Fiz uns testes e vi que funcionou!
</p>

## Aula 2

<p align="justify">
O desafio foi um pouco maior: me transformaram num especialista em achar informações nos seus documentos PDF. Pra isso, precisei acessar seu <strong>Google Drive</strong> (com a sua permissão, claro!). Eu peguei todos os PDFs de uma pasta que você me indicou (<strong>/content/drive/MyDrive/Alura</strong>), li o texto de cada um usando umas ferramentas (<strong>PyMuPDFLoader</strong> e <strong>PyPDF2</strong>) e juntei tudo num arquivo só pra facilitar. Como o texto era grande, eu precisei picar ele em pedacinhos menores (os <strong>"chunks"</strong>). Depois, criei umas representações numéricas desses pedacinhos (os <strong>"embeddings"</strong>) pra eu poder "entender" o que cada um significava. Com esses embeddings, montei um sistema de busca (<strong>FAISS</strong>) pra quando alguém me perguntar algo, eu ir lá e achar os pedacinhos de texto mais parecidos com a pergunta. Me deram um <strong>prompt especial</strong> pra eu usar esses pedacinhos encontrados como <strong>"contexto"</strong> e responder a pergunta apenas com base neles. E pra ficar mais profissional, aprendi a <strong>limpar o texto</strong>, <strong>pegar só os trechos importantes</strong> e <strong>mostrar de onde tirei a informação (as citações!)</strong>. Fiz vários testes e consegui responder algumas perguntas sobre as políticas usando essa base de conhecimento.
</p>

## Aula 3

<p align="justify">
Coloquei tudo junto e me deram uma <strong>"mente"</strong> pra eu decidir o que fazer. Foi com o <strong>langgraph</strong> que isso aconteceu. Eu ganhei um <strong>"estado" (AgentState)</strong> pra guardar todas as informações sobre a conversa (a pergunta, o resultado da triagem, a resposta...). A gente criou um fluxo: primeiro eu passo pela <strong>triagem (o nó node_triagem)</strong>, aí, dependendo do que a triagem disser, eu decido o próximo passo (<strong>decidir_pos_triagem</strong>): se for pra <strong>auto-resolver</strong>, eu vou pro <strong>nó node_auto_resolver</strong>; se for pra <strong>pedir mais info</strong>, vou pro <strong>node_pedir_info</strong>; e se for pra <strong>abrir chamado</strong>, vou pro <strong>node_abrir_chamado</strong>. Depois que eu tento <strong>auto-resolver</strong>, eu verifico se achei a resposta nos seus documentos (<strong>decidir_pos_auto_resolver</strong>). Se sim, termino; se não, vejo se a pergunta tem alguma palavra que indica pra <strong>abrir chamado</strong>, se tiver, abro um, senão, <strong>peço mais informações</strong>. Os nós de <strong>pedir informação</strong> e <strong>abrir chamado</strong> me levam direto pro fim. A gente montou esse desenho todo (<strong>o grafo</strong>!) e eu rodei ele com várias perguntas pra ver se eu seguia o caminho certinho. Foi bem legal ver o fluxo acontecendo e as decisões que eu tomava!
</p>

<p align="justify">
E é isso! Nessas três aulas, eu saí de um <strong>modelo</strong> que só respondia pra um <strong>agente</strong> que consegue <strong>entender a intenção da pergunta</strong>, <strong>buscar a resposta em documentos</strong> e <strong>decidir o melhor jeito de te ajudar</strong>, seja <strong>respondendo direto</strong>, <strong>pedindo mais detalhes</strong> ou <strong>abrindo um chamado</strong> pra você. Foi uma <strong>jornada</strong> e tanto!
</p>

