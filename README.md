% Enunciado de clase
%pariente(Padre, Hijo) -> aridad 2 -> relaciona a un/a padre/madre con un/a hijo/a
pariente(homero, bart).
pariente(homero, lisa).
pariente(homero, maggie).
pariente(marge, bart).
pariente(marge, lisa).
pariente(marge, maggie).
pariente(ned, tod).
pariente(ned, rod).
pariente(abe, homero).
pariente(abe, herbert).

%hermanos(Hermano1, Hermano2).
hermanos(Hermano1, Hermano2):-
    pariente(Pariente, Hermano1),
    pariente(Pariente, Hermano2).

%tio(Tio, Sobrino).
tio(Tio, Sobrino):-
    pariente(Pariente, Sobrino),
    hermanos(Pariente, Tio).

tio(ned, maggie).

%unicoPadre(Hijo) -> no tiene más de uno
unicoPadre(Hijo):-
    pariente(_, Hijo),
    not(tieneMasDeUnPariente(Hijo)).


tieneMasDeUnPariente(Hijo):-
    pariente(Pariente1, Hijo),
    pariente(Pariente2, Hijo),
    Pariente1 \= Pariente2.

% No necesitamos cambiar nada porque por el principio de Universo Cerrado al consultar ya nos daria false.

%quilombero(Persona)
quilombero(bart).
quilombero(homero).

%deportista(Persona)

deportista(rod).
deportista(tod).
deportista(lisa).

%tranqui(Persona).
tranqui(Persona):-
    persona(Persona),
    not(quilombero(Persona)),
    not(deportista(Persona)).

%persona(Persona)
persona(Hijo):-
    pariente(_,Hijo).

persona(Pariente):-
    pariente(Pariente, _).

%esFanDelDeporte(Pariente).
esFanDelDeporte(Pariente):-
    pariente(Pariente, _),
    not(noTieneHijosDeportistas(Pariente)).

noTieneHijosDeportistas(Pariente):-
    pariente(Pariente, Hijo),
    not(deportista(Hijo)).

%Para todo hijo del padre, el hijo es deportista
esFanDelDeporte2(Persona):-
    pariente(Persona, _),
    forall(pariente(Persona, Hijo), deportista(Hijo)).

%noSeBancaHermanos(Persona)
noSeBancaHermanos(Persona):-
    hermanos(Persona, _),
    not(tranqui(Persona)),
    forall(hermanos(Persona, Hermano), tranqui(Hermano)).

%parejaSoniada(Persona, OtraPersona).
%Para todo hijo de UnaPersona -> que sea hijo de OtraPersona
parejaSoniada(UnaPersona, OtraPersona):-
    pariente(UnaPersona, _),
    pariente(OtraPersona, _),
    UnaPersona \= OtraPersona,
    forall(pariente(UnaPersona, Hijo), pariente(OtraPersona, Hijo)).

%forall(Antecedente, Consecuente)

%%%%%29

deporteQueHace(lisa, futbol(15,100,3)).
deporteQueHace(rod, futbol(11,0,100)).

deporteQueHace(lisa, natacion(1200,20)).

deporteQueHace(tod, rugby(pilar,1)).
deporteQueHace(rod, rugby(wings,0)).

%9
quienNada(Persona):-
    deporteQueHace(Persona,natacion(_,_)).

%10

esBuenDeportista(Persona):-
    deporteQueHace(Persona, Deporte),
    esBueno(Deporte).

esBueno(futbol(_,Goles,Out)) :-
    Goles - Out > 5.

esBueno(rugby(Posicion,_)):-
    member(Posicion, [pilar, wings]).

esBuenonatacion(Metros, _):-
      Metros > 1000.

%11

esExitoso(Persona):-
    deporteQueHace(Persona,_), %%para todo deporte
    forall(deporteQueHace(Persona, Deporte), tieneMasdeDiez(Deporte)). %%busque en cada persona, para todo deporte

tieneMasdeDiez(futbol(Medallas,_,_)):-
    Medallas > 10.

tieneMasdeDiez(rugby(_,Medallas)):-
        Medallas > 10.

tieneMasdeDiez(natacion(_,Medallas)):-
        Medallas > 10.
        




