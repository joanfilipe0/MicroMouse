driver = {};
var rastro = 1;
var atual,objetivo,filho;
var listaVisitados = new Array();
var listaVisitar = new Array();
var melhorCaminho = new Array();
var fim = 0;
var parte = 0;
var a;
var rightWall = 0;
 
 
var grafo = new Array(256); //Criando um grafo de 256x256 zerado. Para utilizar algoritmos de busca
for ( var x = 0; x<256; x++) {
    grafo[x] = new Array(256).fill(0);
}
 
var labirinto = new Array(16); //Criando uma matriz de 16x16 que representa o labirinto
for ( var x = 0; x<16; x++) {
    labirinto[x] = new Array(16).fill(0);
}
 
var mPai = new Array(256).fill(500);
 
function testaObjetivo(atual, objetivo) {  //Essa função testa se chegou no objetivo, retornando V ou F
  if(atual == objetivo)
    return true;    
  else
    return false;
}
 
function salvaConexaoGrafo(){
 
    if(mouse.heading() == "N"){   // se o rato tá virado para o norte:
 
           if(mouse.isPathRight() && labirinto[mouse.x()+1][mouse.y()] != 0){ //se rato, estiver olhando para a direita, e não estiver no centro:
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()+1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()+1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathLeft() && labirinto[mouse.x()-1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()-1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()-1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathFwd() && labirinto[mouse.x()][mouse.y()-1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()-1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()-1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathBack() && labirinto[mouse.x()][mouse.y()+1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()+1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()+1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
    }
 
    if(mouse.heading() == "S"){
 
           if(mouse.isPathRight() && labirinto[mouse.x()-1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()-1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()-1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathLeft() && labirinto[mouse.x()+1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()+1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()+1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathFwd() && labirinto[mouse.x()][mouse.y()+1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()+1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()+1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathBack() && labirinto[mouse.x()][mouse.y()-1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()-1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()-1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
    }
 
    if(mouse.heading() == "E"){
 
           if(mouse.isPathRight() && labirinto[mouse.x()][mouse.y()+1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()+1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()+1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathLeft() && labirinto[mouse.x()][mouse.y()-1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()-1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()-1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathFwd() && labirinto[mouse.x()+1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()+1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()+1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathBack() && labirinto[mouse.x()-1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()-1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()-1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
    }
 
    if(mouse.heading() == "W"){
 
           if(mouse.isPathRight() && labirinto[mouse.x()][mouse.y()-1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()-1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()-1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathLeft() && labirinto[mouse.x()][mouse.y()+1] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()][mouse.y()+1]] = 1;
              grafo [labirinto[mouse.x()][mouse.y()+1]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathFwd() && labirinto[mouse.x()-1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()-1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()-1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
           if(mouse.isPathBack() && labirinto[mouse.x()+1][mouse.y()] != 0){
 
              grafo [labirinto[mouse.x()][mouse.y()]][ labirinto[mouse.x()+1][mouse.y()]] = 1;
              grafo [labirinto[mouse.x()+1][mouse.y()]][labirinto[mouse.x()][mouse.y()]] = 1;
           }
    }
}
 
driver.load = function() {  //limpa memoria do rato e vai pra casa
 
    driver.goGoal = true;  // quadrados centrais serão o destino
}
 
var inicio = performance.now(); //função para medir o tempo em milisegundos  ( inicio =0 )
 
driver.next = function() {  // inicia a simulação
    var lval = 255;
    var tempo = 30;   //Tempo para o rato conhecer o labirinto
    var ldir;
    console.log(mouse.memPrintData());  //Imprime os dados do usuário armazenados em cada célula do labirinto.
    fim = performance.now();            //Armazena na variavel fim, o tempo em milissegundos no momento atual
    if ( ((fim-inicio) >= tempo*1050) && parte == 0) {  // fim - inicio em milissegundos >= tempo estipulado
      // Analisa o tempo, e parte como já é 0 (no inicio), já finaliza a busca e parte será 1, proximo estágio
      // onde ... 
        parte = 1;
        mouse.stop();
        alert("Busca realizada:"+mouse.moveCount()+ "\n Executando o melhor caminho: "); // move.count calcula a soma do caminho percorrido
        mouse.home();
        mouse.start();
  
    }
    if (mouse.isGoal() && driver.goGoal) {   // Faz continuar a busca mesmo após chegar no objetivo, para evitar loop
         rightWall = 1;    
         objetivo = labirinto[mouse.x()][mouse.y()];
    }
 
    if (parte == 1){ // Executa a busca em largura
 
        atual = 1; // Sabe que chegou ao objetivo em algum momento
 
    while(true){  //BUSCA EM LARGURA
        
        listaVisitados.push(atual);
    
        a = 0;
 
        while(a<256){ //preenche de lista estados a visitar     
            if(grafo[atual][a] == 1){ //quando acha o num 1 na matriz
                
                listaVisitar.push(a);       
            }
            a++;    
        }
    
        //sucessao      
        for(var i = 0;i<listaVisitados.length;i++){     //Para n repetir um estado
            for(var j = 0;j<listaVisitar.length;j++){
                if(listaVisitar[j] == listaVisitados[i]) 
                    listaVisitar.splice(j, 1);
            }
        }
 
        var copia = listaVisitar.slice();     //Salva quem é pai de quem
        for(var i = 0;i <= copia.length;i++){      //Demarca o nó do grafo para não sobreecrever posteriormente
            if(mPai[copia[copia.length - 1]] == 500)
                mPai[copia[copia.length - 1]] = atual;
            copia.pop();    
        } 
    
        atual = listaVisitar[0];            //sucessao do atual
        listaVisitar.splice(0, 1);
        
        if(testaObjetivo(atual,objetivo)){ //Se alcançou  o objetivo, para!
 
            break;
        }
    
        //Fim sucessao  
    }//FIM DA BUSCA
 
        if(testaObjetivo(atual,objetivo)){
        
        filho = objetivo;
        
        while(true) { 
            if(filho == 500)  // 500 representa estados q n tem pai
                break;
            melhorCaminho.push(filho);
            filho = mPai[filho]; // pai se torna filho;
                 }
                 parte = 2; 
      }
    }
    
    if(parte == 2){ // Utiliza o melhor caminho
 
       if(mouse.heading() == "N"){
          if(mouse.isPathRight() && labirinto[mouse.x()+1][mouse.y()] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' --');
 
            mouse.memPrintData();  
             mouse.right();
             mouse.fwd();    
          }
          else if(mouse.isPathFwd() && labirinto[mouse.x()][mouse.y()-1] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' | ');
 
            mouse.memPrintData();  
             mouse.fwd();       
          }
          else if(mouse.isPathLeft() && labirinto[mouse.x()-1][mouse.y()] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData('-- ');
 
            mouse.memPrintData();  
             mouse.left();
             mouse.fwd();    
          }
       }
 
       else if(mouse.heading() == "S"){
          if(mouse.isPathRight() && labirinto[mouse.x()-1][mouse.y()] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData('-- ');
 
            mouse.memPrintData();  
             mouse.right();
             mouse.fwd();    
          }
          else if(mouse.isPathFwd() && labirinto[mouse.x()][mouse.y()+1] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' | ');
 
            mouse.memPrintData();  
             mouse.fwd();       
          }
          else if(mouse.isPathLeft() && labirinto[mouse.x()+1][mouse.y()] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' --');
 
            mouse.memPrintData();  
             mouse.left();
             mouse.fwd();    
          }
       }
 
       else if(mouse.heading() == "E"){
          if(mouse.isPathRight() && labirinto[mouse.x()][mouse.y()+1] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' | ');
 
            mouse.memPrintData();  
             mouse.right();
             mouse.fwd();    
          }
          else if(mouse.isPathFwd() && labirinto[mouse.x()+1][mouse.y()] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' --');
 
            mouse.memPrintData();  
             mouse.fwd();       
          }
          else if(mouse.isPathLeft() && labirinto[mouse.x()][mouse.y()-1] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' | ');
 
            mouse.memPrintData();  
             mouse.left();
             mouse.fwd();    
          }
       }
 
       else if(mouse.heading() == "W"){
          if(mouse.isPathRight() && labirinto[mouse.x()][mouse.y()-1] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' | ');
 
            mouse.memPrintData();  
             mouse.right();
             mouse.fwd();    
          }
          else if(mouse.isPathFwd() && labirinto[mouse.x()-1][mouse.y()] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData('-- ');
 
            mouse.memPrintData();  
             mouse.fwd();       
          }
          else if(mouse.isPathLeft() && labirinto[mouse.x()][mouse.y()+1] == melhorCaminho[melhorCaminho.length - 1]){
            mouse.memSetData(' | ');
 
            mouse.memPrintData();  
             mouse.left();
             mouse.fwd();    
          }
       }
       melhorCaminho.pop();
       
  if (mouse.isGoal() && driver.goGoal) {
        alert("Centro encontrado: "+mouse.moveCount()+"\nVolte Para o Home.");
        driver.goGoal = !driver.goGoal;
        
    }
       return;
    }
 
    if (rightWall == 1) {
      
      if (mouse.isPathRight()) {
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.right();
            mouse.fwd();
      } 
      else if (mouse.isPathFwd()) {
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.fwd();
      } 
      else {
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.left();
            mouse.fwd();
        }
      return;
    }
 
    // get the current flood values.
    mouse.memFlood(driver.goGoal);
 
    if (mouse.isPathFwd() ) {
        lval = mouse.memGetDataFwd();
        ldir = "F";
    }
 
    if (mouse.isPathLeft() ) {
        if (mouse.memGetDataLeft() < lval) {
            lval = mouse.memGetDataLeft();
            ldir = "L";
        }
    }
 
    if (mouse.isPathRight() ) {
        if (mouse.memGetDataRight() < lval) {
            lval = mouse.memGetDataRight();
            ldir = "R";
        }
    }
 
    if (mouse.isPathBack() ) {
        if (mouse.memGetDataBack() < lval) {
            lval = mouse.memGetDataBack();
            ldir = "B";
        }
    }
 
 
    switch (ldir) {
        case "F" :
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.fwd();
            break;
        case "L" :
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.left();
            mouse.fwd();
            break;
        case "R" :
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.right();
            mouse.fwd();
            break;
        case "B" :
             if(labirinto[mouse.x()][mouse.y()] == 0){
               labirinto[mouse.x()][mouse.y()] = rastro;
               rastro++;
            }
            salvaConexaoGrafo();
            mouse.left(2);
            mouse.fwd();
            break;
    }
 
}
 
 
 

