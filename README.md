# 8-puzle
#include <iostream>
#include <stdlib.h>
#include <conio.h>
#include <math.h>
#include <vector>
#include <queue>


using namespace std;
class Node
{
public:

    Node * pai;
    Node * filho1;
    Node * filho2;
    Node * filho3;
    Node * filho4;

    int espi;
    int espj;
    int A[3][3];
    int G;
    int H;
    bool visited;
    Node()
    {
        pai = NULL;
        filho1 = NULL;
        filho2 = NULL;
        filho3 = NULL;
        filho4 = NULL;
        visited = false;
        G=0;
        H=0;
    }
};
bool repeating(Node *F,vector <Node*> position,  vector <int> value);
void achezero(Node &N);
void estrela(Node &M);
int acheg(Node &N);
void copiar(int (&A)[3][3],int (&B)[3][3]);
void imprimir(Node &N);
int main()
{
    int i, j;
    Node raiz;
    cout << "Digite a  matriz" << endl;
    for(i=0; i<=2; i++)
    {
        for(j=0; j<=2; j++)
        {
             raiz.A[i][j] = (_getch()- '0');
             cout << raiz.A[i][j];
        }
        cout << endl;
    }
    estrela(raiz);
    return 0;
}
void estrela(Node &M)
{
    int G, i;
    priority_queue<int> q;
    vector<Node*> position;
    vector<int> value;
    achezero(M);
    G = acheg(M);
    Node * N = &M;
    while(G>0)
    {
        N->visited = true;

        if(N->espi>0&&N->filho1==NULL)
        {
            Node *F;
            F = new Node;
            N->filho1 = F;
            F->pai = N;
            copiar(N->A,F->A);
            F->espi = N->espi;
            F->espj = N->espj;
            int temp = F->A[(F->espi)-1][F->espj];
            F->A[(F->espi)-1][F->espj] = 0;
            F->A[F->espi][F->espj] = temp;
            achezero(*F);
            F->G = acheg(*F);
            F->H = (N->H)+1;
            if(!repeating(F, position, value))
            {
                position.push_back(F);
                value.push_back((F->G)+(F->H));
            }

        }
        if(N->espj<2&&N->filho2==NULL)
        {
            Node *F;
            F = new Node;
            N->filho2 = F;
            F->pai = N;
            copiar(N->A,F->A);
            F->espi = N->espi;
            F->espj = N->espj;
            int temp = F->A[F->espi][F->espj+1];
            F->A[F->espi][F->espj+1] = 0;
            F->A[F->espi][F->espj] = temp;
            achezero(*F);
            F->G = acheg(*F);
            F->H = (N->H)+1;
            if(!repeating(F, position, value))
            {
                position.push_back(F);
                value.push_back((F->G)+(F->H));
            }
        }
        if(N->espi<2&&N->filho3==NULL)
        {

            Node *F;
            F = new Node;
            N->filho2 = F;
            F->pai = N;
            copiar(N->A,F->A);
            F->espi = N->espi;
            F->espj = N->espj;
            int temp = F->A[(F->espi)+1][F->espj];
            F->A[(F->espi)+1][F->espj] = 0;
            F->A[F->espi][F->espj] = temp;
            achezero(*F);
            F->G = acheg(*F);
            F->H = (N->H)+1;
            if(!repeating(F, position, value))
            {
                position.push_back(F);
                value.push_back((F->G)+(F->H));
            }
        }
        if(N->espj>0&&N->filho4==NULL)
        {
            Node *F;
            F = new Node;
            N->filho2 = F;
            F->pai = N;
            copiar(N->A,F->A);
            F->espi = N->espi;
            F->espj = N->espj;
            int temp = F->A[F->espi][(F->espj)-1];
            F->A[F->espi][(F->espj)-1] = 0;
            F->A[F->espi][F->espj] = temp;
            achezero(*F);
            F->G = acheg(*F);
            F->H = (N->H)+1;
            if(!repeating(F, position, value))
            {
                position.push_back(F);
                value.push_back((F->G)+(F->H));
            }
        }
        /*
        for(i=0; i <  (int)value.size(); i++)
        {
            if(N == position[i])
            {
                position.erase(position.begin() + i);
                value.erase(value.begin() + i);

            }

        }
        */
        int maxF = 9999999;
        for(i=0; i <  (int)value.size(); i++)
        {
            if(maxF > value[i]&&!(position[i]->visited))
            {
                N = position[i];
                maxF = value[i];
            }

        }
        cout << position.size() << endl;
        G = acheg(*N);
/*
        cout << "---------------------" << endl;
        cout << "depth: " << N->H << endl;
     imprimir(*N);
        cout << "---------------------" << endl;
     cout << endl;
     */
/*
     for(i=0; i < (int)value.size(); i++)
     {
         Node * X;
         X = position[i];
         imprimir(*X);
         cout << endl;
     }
     */
    }
    N = N->pai;
    do
    {
        cout << "        ^            " << endl;
        cout << "        |            " << endl;
        cout << "---------------------" << endl;
        cout << "depth: " << N->H << endl;
     imprimir(*N);
        cout << "---------------------" << endl;
     cout << endl;
        N = N->pai;
    }while(N != &M);
        cout << "        ^            " << endl;
        cout << "        |            " << endl;
        cout << "---------------------" << endl;
        cout << "depth: " << 0<< endl;
     imprimir(M);
        cout << "---------------------" << endl;
     cout << endl;

    return;
}

void achezero(Node &N)
{
    int i, j;
    for(i=0; i<=2; i++)
    {
        for(j=0; j<=2; j++)
        {
             if(N.A[i][j] == 0)
             {
                N.espi = i;
                N.espj = j;
             }
        }
    }
    return;
}
int acheg(Node &N)
{
    int i, j;
    N.G = 0;
    int X0[9] = {2,0,0,0,1,1,1,2,2};
    int Y0[9] = {2,0,1,2,0,1,2,0,1};
    for(i=0; i<=2; i++)
    {
        for(j=0; j<=2; j++)
        {
            if(N.A[i][j]!=0/*&&X0[N.A[i][j]-1]!=i&&Y0[N.A[i][j]-1]!=i*/)
             //N.G = N.G + 1;
                N.G = N.G + ( abs(i-X0[(N.A[i][j])])+ abs(j-Y0[(N.A[i][j])]));
        }
    }

    return N.G;

}

void imprimir(Node &N)
{
    int i, j;
    for(i=0; i<=2; i++)
    {
        for(j=0; j<=2; j++)
        {
             cout << N.A[i][j];
        }
        cout << endl;
    }
    return;
}

void copiar(int (&A)[3][3],int (&B)[3][3])
{
    int i, j;

    for(i=0; i<=2; i++)
    {
        for(j=0; j<=2; j++)
        {
             B[i][j] = A[i][j];
        }
    }
    return;
}

bool repeating(Node *F,vector <Node*> position, vector <int> value)
{

    int i, j, z;
    bool repeated=false;
    Node *X;
    for(z=0; z <  (int)position.size(); z++)
    {
        X = position[z];
        if(F->G == X->G)
        {
            bool dif = false;
            for(i=0; i<=2; i++)
            {
                for(j=0; j<=2; j++)
                {
                    if(F->A[i][j]!=X->A[i][j])
                    {
                        dif = true;
                        break;
                    }
                }
                if(dif==true)
                {
                    break;
                }
            }
            if(dif==false)
            {
                repeated = true;
                break;
            }
        }
    }
    if (repeated == true)
    {

        if(F->H < X->H)
        {
            position[z] = F;
            value[z] = F->H + F->G;
        }

        return true;
    }
    else
        return false;
}
