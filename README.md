# ProyectoFund.Programacion
Es un juego con 2 jugadores en un tablero, y el objetivo es ganar el control del tablero atacando a los blobs de tu oponente y convirtiéndolos a blobs tuyos.
//
//Autor: Doris Garcia
//Matricula: A01282287
//Fecha:18/11/2016
//
//TareaIntegradora
//

#include <iostream>
#include <cctype>
using namespace std;

//Función para mostrar tablero

void vFuncionMuestraTablero(char cMatriz[5][5])
{
    cout<<"   0  1  2  3  4"<<endl;

    for(int iReng = 0; iReng < 5; iReng++)
    {
        cout << iReng << "  ";
        for(int iColumna = 0; iColumna < 5; iColumna++)
        {
            cout <<cMatriz[iReng][iColumna]<<"  ";
        }
        cout << endl;
    }
}
//Se inicia tablero
void vFuncionInicioTablero(char cTablero[5][5])
{
    for(int iReng = 0; iReng < 5; iReng++)
    {
        for(int iColumna = 0; iColumna < 5; iColumna++)
        {
            cTablero[iReng][iColumna]= '~';

        }
    }
    cTablero[0][0] = 'X';
    cTablero[0][4] = 'X';
    cTablero[4][0] = 'O';
    cTablero[4][4] = 'O';
}

//Transforma blobs oponentes en celdas adyacentes a propios
void FuncionTransformaBlob(char cMatriz[5][5], int iColumna, int iRenglon, char cBlob) {

    if (iRenglon>4 || iRenglon<0 || iColumna>4 || iColumna<0  || cMatriz[iRenglon][iColumna]=='~')
        return;

    cMatriz[iRenglon][iColumna]=cBlob;

}

//Función que establece el movimiento de blobs dependiendo el caso
void vFuncionMovimiento(char cMatriz[5][5], int iRenglon, int iColumna, char cMovimiento)
{
    char cBlob=cMatriz[iRenglon][iColumna];


    switch (cMovimiento) {
        case 'A': iColumna--;
            break;
        case 'X': iRenglon++;
            break;
        case 'W': iRenglon--;
            break;
        case 'D' : iColumna++;
            break;
        case 'Q' : iRenglon--; iColumna--;
            break;
        case 'E' : iRenglon--; iColumna++;
            break;
        case 'Z' : iRenglon++; iColumna--;
            break;
        case 'C' : iRenglon++; iColumna++;
            break;
    }
    if (cBlob=='X') {
        if (iRenglon>4 || iRenglon<0 || iColumna>4 || iColumna<0  || (cMatriz[iRenglon][iColumna]!='~' && cMatriz[iRenglon][iColumna]!='O'))
            return;
    }else if (cBlob=='O'){
        if (iRenglon>4 || iRenglon<0 || iColumna>4 || iColumna<0  || (cMatriz[iRenglon][iColumna]!='~' && cMatriz[iRenglon][iColumna]!='X'))
            return;
    }


    cMatriz[iRenglon][iColumna]=cBlob;

    FuncionTransformaBlob(cMatriz, iColumna+1, iRenglon, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna+1, iRenglon+1, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna, iRenglon+1, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna-1, iRenglon+1, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna-1, iRenglon, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna-1, iRenglon-1, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna, iRenglon-1, cBlob);
    FuncionTransformaBlob(cMatriz, iColumna+1, iRenglon-1, cBlob);
}
//Función para determinar ganador del juego

void vFuncionCuentaGanador(char cMatriz [5][5])
{
    int iZ=0, iQ=0;

    for(int iRenglon = 0; iRenglon < 5; iRenglon++)
    {
        for(int iColumna = 0; iColumna < 5; iColumna++)
        {
            if(cMatriz[iRenglon][iColumna]=='X')
            {
                iZ++;
            }
            if(cMatriz[iRenglon][iColumna]=='O')
            {
                iQ++;
            }

        }


    }

    cout<<"Jugador 1 (X) ->"<<iZ++<<endl;
    cout<<"Jugador 2 (O) ->"<<iQ++<<endl;

    if (iZ>iQ)
        cout<<"Gana jugador X"<<endl;
    if (iQ>iZ)
        cout<<"Gana jugador O"<<endl;
    if (iQ==iZ)
        cout<<"Los dos ganaron"<<endl;

}

int main(){
    char cMatriz[5][5], cMovimiento, cSeguir, cBlob='X';
    int iRenglon, iColumna;

    vFuncionInicioTablero(cMatriz);
    vFuncionMuestraTablero(cMatriz);
    do {
        cBlob='X';
        cout << "Jugador X " << endl;

        do {
            cout << "Renglon --> " << endl;
            cin >> iRenglon;
            cout << "Columna --> " << endl;
            cin >> iColumna;
        } while (cMatriz[iRenglon][iColumna]!= cBlob);


        cout<<"a - izquierda"<<" "<<"x - abajo" << endl<< "w - arriba" << " " << "d - derecha" << endl <<"q - arriba a la izquierda"<<" "<<"e - arriba a la derecha"<<endl<<"z - abajo a la izquierda"<<" "<<"c - abajo a la derecha"<<endl;
        do{
            cout<<"Movimiento = ";
            cin>>cMovimiento;

            cMovimiento = toupper (cMovimiento);

        }while(cMovimiento!='A' && cMovimiento!='X' && cMovimiento!='W' && cMovimiento!='D' && cMovimiento!='Q' && cMovimiento!='E' && cMovimiento!='Z' && cMovimiento!='C');

        vFuncionMovimiento(cMatriz, iRenglon, iColumna, cMovimiento);
        vFuncionMuestraTablero(cMatriz);

        do{
            cout<<"Quieres seguir jugando (s/n)?";
            cin>> cSeguir;
            cSeguir = toupper (cSeguir);

        }while(cSeguir!='S' && cSeguir!='N');
        if(cSeguir=='N'){
            break;
        }
        cBlob='O';

        cout << "Jugador O " << endl;

        do {
            cout << "Renglón --> " << endl;
            cin >> iRenglon;
            cout << "Columna --> " << endl;
            cin >> iColumna;
        } while (cMatriz[iRenglon][iColumna]!=cBlob);

        cout<<"a - izquierda"<<" "<<"x - abajo" << endl<< "w - arriba" << " " << "d - derecha" << endl <<"q - arriba a la izquierda"<<" "<<"e - arriba a la derecha"<<endl<<"z - abajo a la izquierda"<<" "<<"c - abajo a la derecha"<<endl;

        do{
            cout<<"Movimiento = ";
            cin>>cMovimiento;

            cMovimiento = toupper (cMovimiento);

        }while(cMovimiento!='A' && cMovimiento!='X' && cMovimiento!='W' && cMovimiento!='D' && cMovimiento!='Q' && cMovimiento!='E' && cMovimiento!='Z' && cMovimiento!='C');

        vFuncionMovimiento(cMatriz, iRenglon, iColumna, cMovimiento);
        vFuncionMuestraTablero(cMatriz);

        do{
            cout<<"Quieres seguir jugando (s/n)?";
            cin>> cSeguir;
            cSeguir = toupper (cSeguir);

        }while(cSeguir!='S' && cSeguir!='N');
        if(cSeguir=='N'){
            break;
        }
    } while (cSeguir=='S');

    vFuncionCuentaGanador(cMatriz);
}
