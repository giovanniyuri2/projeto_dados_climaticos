# Email professor
O objetivo principal nesse projeto será quantificar como as correlações entre séries temporais de temperatura ao redor do globo variam ao longo do tempo, e como essas informações podem ser utilizadas para detectar anomalias climáticas. 

Para se ambientarem com o problema, sugiro que leiam o artigo no anexo. Não se preocupem em entender 
todos os detalhes; o que é importante para vocês são as seções III (o problema) e a seção V. A. (Materiais e métodos).

As séries temporais com que irão trabalhar estão no arquivo air.mon.mean.nc, que você ser baixado por aqui: 

https://psl.noaa.gov/repository/entry/show?entryid=synth%3Ae570c8f9-ec09-4e89-93b4-babd5651e7a9%3AL25jZXAucmVhbmFseXNpcy5kZXJpdmVkL3N1cmZhY2UvYWlyLm1vbi5tZWFuLm5j

Esse arquivo está num formato especial, chamado netCDF. Nos links abaixo vocês encontram tutoriais úteis sobre como ler esse tipo de arquivo em Python: 

https://towardsdatascience.com/read-netcdf-data-with-python-901f7ff61648
https://stackoverflow.com/questions/36360469/read-nc-netcdf-files-using-python

Outra biblioteca que pode ajudar bastante é a chamada pyunicorn, que é dedicada a criar grafos a partir de séries temporais climáticas -- isso pode poupar bastante trabalho na hora de criar as redes. Deem uma olhada nesse 
tutorial: 

http://www.pik-potsdam.de/~donges/pyunicorn/tutorials/climate_network_1.html