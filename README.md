# Imersão-Agentes-de-IA---Alura + Google-Gemini

## ⚠️ Aviso Importante: Chave de API do Google Gemini

<p align="justify">Para executar este projeto, você precisará de uma chave de API do Google AI Studio.</p>

*   <strong>Obtenha sua chave aqui:</strong> <p align="justify">Visite o [Google AI Studio](https://aistudio.google.com/app/apikey) para gerar sua chave.</p>

*   <strong>Segurança:</strong> É <strong>altamente recomendado</strong> que você utilize métodos seguros para armazenar e acessar sua chave, como os <strong>Google Colab Secrets</strong> (o ícone de chave 🔑 na barra lateral esquerda do Colab) ou variáveis de ambiente, <strong>evitando</strong> inseri-la diretamente no código-fonte, especialmente se for compartilhar o notebook ou usá-lo em produção. O notebook inclui uma opção para inserir a chave diretamente para testes rápidos, mas use com cautela.

## 📄 Utilizando seus Próprios Documentos PDF

<p align="justify">Este notebook foi modificado para permitir que você carregue seus próprios arquivos PDF diretamente no ambiente do Google Colab no momento da execução.</p>

*   <p align="justify">Quando chegar na seção de carregamento de documentos, o notebook solicitará o upload dos arquivos.</p>
*   <p align="justify">Clique no botão de upload e selecione os arquivos PDF que contêm as informações que você deseja que o modelo utilize (por exemplo, políticas internas da sua empresa).</p>
*   <p align="justify">Os documentos de exemplo que estavam no Google Drive foram removidos do fluxo principal para facilitar o uso.</p>


## Aula 1

<p align="justify">Primeiro, instalei umas ferramentas (langchain, langchain-google-genai, google-generativeai) pra eu poder conversar com o Google Gemini. Aí, configurei minha chave pra ter acesso (aquela que você pegou no Google IA Studio e colocou aqui no Colab). Depois, criei uma instância do modelo gemini-2.5-flash, configurei ele pra ser bem direto nas respostas e fiz um teste pra ver se a gente tava se entendendo. O mais legal foi quando me ensinaram a classificar as mensagens dos usuários: eu aprendi a analisar se a pergunta era simples de responder ('AUTO_RESOLVER'), se faltava informação ('PEDIR_INFO') ou se era algo mais complexo que precisava abrir um chamado ('ABRIR_CHAMADO'), e ainda a definir a urgência. Pra garantir que eu classificasse tudo certinho, me deram um "molde" de resposta (TriagemOut) pra eu sempre seguir. Fiz uns testes e vi que funcionou!</p>

## Aula 2

<p align="justify">O desafio foi um pouco maior: me transformaram num especialista em achar informações nos seus documentos PDF. Pra isso, precisei acessar seu Google Drive (com a sua permissão, claro!). Eu peguei todos os PDFs de uma pasta que você me indicou (/content/drive/MyDrive/Alura), li o texto de cada um usando umas ferramentas (PyMuPDFLoader e PyPDF2) e juntei tudo num arquivo só pra facilitar. Como o texto era grande, eu precisei picar ele em pedacinhos menores (os "chunks"). Depois, criei umas representações numéricas desses pedacinhos (os "embeddings") pra eu poder "entender" o que cada um significava. Com esses embeddings, montei um sistema de busca (FAISS) pra quando alguém me perguntar algo, eu ir lá e achar os pedacinhos de texto mais parecidos com a pergunta. Me deram um prompt especial pra eu usar esses pedacinhos encontrados como "contexto" e responder a pergunta apenas com base neles. E pra ficar mais profissional, aprendi a limpar o texto, pegar só os trechos importantes e mostrar de onde tirei a informação (as citações!). Fiz vários testes e consegui responder algumas perguntas sobre as políticas usando essa base de conhecimento.</p>

## Aula 3

<p align="justify">Coloquei tudo junto e me deram uma "mente" pra eu decidir o que fazer. Foi com o langgraph que isso aconteceu. Eu ganhei um "estado" (AgentState) pra guardar todas as informações sobre a conversa (a pergunta, o resultado da triagem, a resposta...). A gente criou um fluxo: primeiro eu passo pela triagem (o nó node_triagem), aí, dependendo do que a triagem disser, eu decido o próximo passo (decidir_pos_triagem): se for pra auto-resolver, eu vou pro nó node_auto_resolver; se for pra pedir mais info, vou pro node_pedir_info; e se for pra abrir chamado, vou pro node_abrir_chamado. Depois que eu tento auto-resolver, eu verifico se achei a resposta nos seus documentos (decidir_pos_auto_resolver). Se sim, termino; se não, vejo se a pergunta tem alguma palavra que indica pra abrir chamado, se tiver, abro um, senão, peço mais informações. Os nós de pedir informação e abrir chamado me levam direto pro fim. A gente montou esse desenho todo (o grafo!) e eu rodei ele com várias perguntas pra ver se eu seguia o caminho certinho. Foi bem legal ver o fluxo acontecendo e as decisões que eu tomava!</p>

<p align="justify">E é isso! Nessas três aulas, eu saí de um modelo que só respondia pra um agente que consegue entender a intenção da pergunta, buscar a resposta em documentos e decidir o melhor jeito de te ajudar, seja respondendo direto, pedindo mais detalhes ou abrindo um chamado pra você. Foi uma jornada e tanto!</p>
