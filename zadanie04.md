172.20.0.1 - - [28/Oct/2026 21:42:21] GET /health HTTP/1.1 200 -
{status:ok}


breach-checker % docker run --rm --network breach-net healthcheck-probe
backend: OK
breach-checker % docker run --rm healthcheck-probe
backend: UNREACHABLE
breach-checker % docker stop backend
backend
breach-checker % docker run --rm --network breach-net healthcheck-probe
backend: UNREACHABLE
breach-checker % docker start backend                                  
backend
breach-checker % docker run --rm --network breach-net healthcheck-probe
backend: OK

#dlaczego bez --network nazwa backend nie działa, choć backend jest uruchomiony?

Bez --network nazwa backend trafia do domyślnej sieci bridge, a nie breach-net w ktorej jest backend. dockerowy DNS nie dziala w domyslnej sieci bridge dockera wiec nie bedzie wiedzial gdzie to zapytanie ma trafic.


breach-checker % docker exec backend sh -c "cat /tmp/notatka.txt"
notatka

futurama@Mac breach-checker % docker exec backend cat /tmp/notatka.txt
cat: /tmp/notatka.txt: No such file or directory

breach-checker % docker run --rm -v breach-dane:/dane alpine sh -c "date >> /dane/historia.txt; cat /dane/historia.txt"
Wed Oct 28 22:14:10 UTC 2026
Wed Oct 28 22:14:59 UTC 2026

plik w /tmp przetrwał restart, ale nie rm dlatego ze przy rm zostal usuniety caly kontener wraz z wartswa z plikami. Natomiast w wypadku historia.txt plik nie był zapisany w warstwie żadnego z kontenerów tylko w wolumenie. docker nie usunie sam z siebie wolumenu przy usunieciu kontenera. LIsta wyciekow przetrwalaby usuniecie bo zostala dodana do wolumenu.