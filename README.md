#include <stdlib.h>
#include <iomanip>
#include <iostream>
using namespace std;


int main() {
	
	float N,S,I=1,R=0,dS=0,dI=0,dR=0,t=0,r0,D,a,b,m,M=0,dM=0,dId,pico,CAPH,CAPI,EXH,EXI,DMDH,DMDI,HOSP,INT,probh,probi,PVH, PVI, MNE;
	
	cout<<"Parametros do modelo\n\n";
	cout<<"Populacao (milhoes): ";
	cin>>N;
	N=N*1000000;
	S=N-I;
	
	cout<<"Taxa basica de reproducao do virus (1+ a 18)(r0): ";
	cin>>r0;
	if(r0<=1||r0>18){
	cout<<"\nEste valor precisava estar entre 1+ a 18\n\n ";
	system ("pause");
	return 0;
	}
	
	cout<<"Duracao media das infeccoes (dias)(>0): ";
	cin>>D;
	if(D<0){
	cout<<"\nEste valor precisava ser maior do que zero\n\n ";
	system ("pause");
	return 0;
	}
	a=1/D;
	b=a*r0;
	
	cout<<"Capacidade do sistema de saude, internacoes nao-intensivas (mil leitos)(>0): ";
	cin>>CAPH;
	CAPH=CAPH*1000;
	if(CAPH<0){
	cout<<"\nEste valor precisava ser maior do que zero\n\n ";
	system ("pause");
	return 0;
	}
	
	cout<<"Capacidade do sistema de saude, internacoes intensivas (mil leitos)(>0): ";
	cin>>CAPI;
	CAPI=CAPI*1000;
	if(CAPI<0){
	cout<<"\nEste valor precisava ser maior do que zero\n\n ";
	system ("pause");
	return 0;
	}
	
	cout<<"Probabilidade de um infectado necessitar de internacao nao-intensiva (%)(0 a 100): ";
	cin>>probh;
	probh=probh/100;
	if(probh<0||probh>1){
	cout<<"\nEste valor precisava estar entre 0 e 100\n\n ";
	system ("pause");
	return 0;
	}
	
	cout<<"Probabilidade de um infectado necessitar de internacao intensiva (%)(0 a 100): ";
	cin>>probi;
	probi=probi/100;
	if(probi<0||probi>1){
	cout<<"\nEste valor precisava estar entre 0 e 100\n\n ";
	system ("pause");
	return 0;
	}
	
	cout<<"Sobrevivencia esperada de um paciente em internacao nao-intensiva (%)(0 a 100): ";
	cin>>PVH;
	PVH=PVH/100;
	if(PVH<0||PVH>1){
	cout<<"\nEste valor precisava estar entre 0 e 100\n\n ";
	system ("pause");
	return 0;
	}
	
	cout<<"Sobrevivencia esperada de um paciente em internacao intensiva (%)(0 a 100): ";
	cin>>PVI;
	PVI=PVI/100;
	if(PVI<0||PVI>1){
	cout<<"\nEste valor precisava estar entre 0 e 100\n\n ";
	system ("pause");
	return 0;
	}
	
	while(I>=1){
		dId=dI;

		DMDH=probh*I;
		DMDI=probi*I;
			if(DMDH>CAPH){
				EXH=DMDH-CAPH;
				HOSP=CAPH;
			}
			else
			EXH=0;
			HOSP=DMDH;
			
			if(DMDI>CAPI){
				EXI=DMDI-CAPI;
			}
			else
			EXI=0;
			INT=DMDI;
		
		m=(EXH+(1-PVH)*HOSP)/I+(EXI+(1-PVI)*INT)/I;
		
		dS=-(b*S*I)/N;
		dI=(b*I*S)/N-a*I;
		dR=a*I*(1-m);
		dM=a*I*m;
		
		S=S+dS;
		I=I+dI;
		M=M+dM;
		R=R+dR;
		N=S+I+R;
		t++;
		if(dId>0&&dI<0){
			pico=I;
		}
	 
	}
	
	MNE=(R+M+I)*probh*(1-PVH)+(R+M+I)*probi*(1-PVI);
	
	cout<<fixed<<setprecision(2);
	cout<<"\nResultados:\n\n";
	cout<<"Duracao da epidemia (primeiro ao ultimo caso): "<<t<<" dias\n";
	cout<<"Populacao que contraiu o virus no fim da epidemia: "<<(R+M+I)/1000000<<" milhoes, " <<(100*(R+M+I))/(N+M+I)<<"% do total\n";
	cout<<"Populacao infectada no auge da epidemia: "<<pico/1000000<<" milhoes, "<<(100*pico)/N<<"% do total\n";;
	cout<<"Demanda por internacoes nao intensivas relativa aos leitos disponiveis no auge da epidemia: "<<(pico*probh)/CAPH<<"\n";
	cout<<"Demanda por internacoes intensivas relativa aos leitos disponiveis no auge da epidemia: "<<(pico*probi)/CAPI<<"\n";
	cout<<"Mortes totais ao final da epidemia: "<<M/1000<<" mil\n";
	cout<<"Mortes causadas pela escassez no sistema de saude: "<<M/1000-MNE/1000<<" mil, "<<(M-MNE)*100/M<<"% do total\n\n";
	
	system("pause");
	return 0;
	
}
