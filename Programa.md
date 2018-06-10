;# Area
;Programa em Assembly que Calcula a Areá de um retângulo, trapézio ou triangulo 
%include "io.inc"

;macro para imprimir texto
%macro imprimirtexto 1
push %1     
call printf 
add esp,4   
%endmacro

;macro para imprimir resultado final
%macro imprimirresult 1
 push eax    
 push %1     
 call printf 
 add esp, 8  
%endmacro

SECTION .data
       msginicio:    db "#############CALCULAR AREA#####################", 10, 0
       askTriangulo: db 'Digite 1 Para calcular area do Triangulo', 10, 0.
       askTrapezio:  db 'Digite 2 Para calcular area do Trapezio',10, 0.
       askRetangulo: db 'Digite 3 para calcular area do Retangulo',10, 0.
       askBase:      db 'Entre com o valor da Base',10, 0.
       askBaseMaior: db 'Entre com o valor da Maior Base',10, 0.
       askBaseMenor: db 'Entre com o valor da Menor Base',10, 0.
       askAltura:    db 'Entre com o valor da Altura',10, 0.
       resTriangulo: db "A area do triangulo e %d" ,10 , 0.
       resTrapezio:  db "A area do trapezio e %d" ,10 , 0.
       resRetangulo: db "A area do retangulo e %d" ,10 , 0.
       msgFinal:     db "###################FIM##################",10, 0
       formatin: db "%d",0
       
       
       escolha: times 4 db 0 ; guarda a opção escolhida
       
       ;variaveis para guardar valores
                area: times 4 db 0
       maiorbase: times 4 db 0
           menorbase: times 4 db 0
              altura: times 4 db 0
      
section .text
extern scanf
extern printf
    
global CMAIN
CMAIN:
    mov ebp, esp
    ;imprimindo o menu na tela
    imprimirtexto msginicio
    imprimirtexto askTriangulo
    imprimirtexto askTrapezio
    imprimirtexto askRetangulo
    
    
    push escolha
    push formatin
    call scanf
    add esp, 8
    
    ;compara o valor escolhido
    mov ebx, [escolha]
    
    ;caso escolha numero 1
    jmp else_block1

     if_block1: 
      
       ;armazena o valor da base
       imprimirtexto askBase
       push maiorbase
       push formatin
       call scanf
       add esp, 8
       
       ;armazena o valor da altura
       imprimirtexto askAltura
       push altura
       push formatin
       call scanf
       add esp, 8
      
      ;calculo triangulo
      call triangulo
      
      ;imprime o resultado na tela
      imprimirresult resTriangulo
      
      ;chama macro para imprimir mensagem de saida do console
      imprimirtexto msgFinal
      push formatin
      call scanf
      add esp, 8
          
      jmp end_if
      
      else_block1:
           ;caso for 1
           cmp ebx, 1  
           je if_block1 
           jmp else_block2 
           
     if_block2:
     
        ;pedindo para entrar com o valor da maior base
       imprimirtexto askBaseMaior
       push maiorbase
       push formatin
       call scanf
       add esp, 8
       
       ;pedindo para entrar com o valor da menor base
       imprimirtexto askBaseMenor
       push menorbase
       push formatin
       call scanf
       add esp, 8
       
       ;pedindo para entrar valor da altura
       imprimirtexto askAltura
       push altura
       push formatin
       call scanf
       add esp, 8
       
      ;area do trapezio
      call trapezio
      
      ;exibe o resultado na tela
      imprimirresult resTrapezio
      
      imprimirtexto msgFinal
      push formatin
      call scanf
      add esp, 4
      
     jmp end_if
          else_block2:
           ;comparando o valor com a opção 2
           cmp ebx, 2
           je if_block2 
           jmp else_block3 
           
     
     if_block3:  
      
       ;pedindo para entrar valor da base
       imprimirtexto askBase
       push maiorbase
       push formatin
       call scanf
       add esp, 8
       
       ;pedindo para entrar valor da altura
       imprimirtexto askAltura
       push altura
       push formatin
       call scanf
       add esp, 8
       
       ;calcular retangulo
       call retangulo
      
       ;imprime o resultado na tela
       imprimirresult resRetangulo
       
      imprimirtexto msgFinal
      
      push formatin
      call scanf
      add esp, 4
       
      jmp end_if
           else_block3:
           ;comparando o valor com opção 3
           cmp ebx, 3
           je if_block3 
           jmp else_block1 
     end_if:
      
     
    xor eax, eax
    ret
    
;calcular triangulo    
triangulo:     
        mov ebx,[maiorbase]     ;armazena o valor da base
        mov eax,[altura]        ;armazena o valor da altura
        mul ebx                 ;eax = b * a  
        mov ebx, 2              ;divide por 2
        div ebx                 ;ebx = eax/2;
        
        mov [area], eax         
        ret 

;calcular retangulo
retangulo:

        mov ebx,[maiorbase]     ;manda valor escolhido da base
        mov eax,[altura]        ;manda valor escolhido da altura
        mul ebx                 ;eax = b * a      
        mov ebx, 2              ;diz que valor sera dividido por 2
        div ebx                 ;ebx = eax/2;
        
        mov [area], eax         ;joga valor do eax no [area] para saida
        ret     
        
;calcular trapezio           
trapezio:
        mov eax,[menorbase]     ;armazena da menor base
        mov ebx,[maiorbase]     ;armazena resultado na base menor
        add eax, ebx            ;eax = (bma + bme)     
        mov ecx,[altura]        ;manda valor escolhido da altura
        mul ecx,                ;ecx = ecx * eax
        mov ecx, 2              ;diz que valor sera dividido por 2
        div ecx,                ;eax = eax/2
        
        mov [area], eax         
   
        ret
