I - INSTALACJA:
1.1. Zaleca się pobranie plików do /usr/local/bin/etherspeed-check/. W przypadku instalacji w innym katalogu należy zmienić wartośc workdir w pliku etherspeed-check.

1.2. W przypadku, gdy chcemy logować się za pomocą hasła należy doinstalowac  pakiet sshpass (apt-get install sshpass)

1.3. Konfiguracja SSH pubkey. W przypadku korzystania z sshpass możemy przejśc do kroku 4-go.

1.3.1. Najpierw musimy wygenerowac klucz dla użytkownika systemu linuxowego poleceniem:
ssh-keygen -t rsa
Następnie plik z katalogu ~/.ssh/id_rsa.pub pobieramy na lokalny komputer, np. za pomocą SFTP)

1.3.2 Export do Ubiquiti
    Logujemy się do urządzenia. Wchodzimy w zakładkę Services > Serwer SSH > Uprawnione klucze: > [Edytuj...]
    Importuj plik klucza publicznego: Podajemy lokalizację pliku id_rsa.pub (z dysku lokalnego)
    Klikamy [Importuj] i [Zapisz].
    Małe okienko powinno się zamknąć.
    Następmnie [Zmień] i [Zastosuj].
    Po około minucie dostęp do urządzenia powinien być dostępny za pomocą SSH z metodą uwierzytelniania pubkey (bez hasła)

1.3.3 Export do MikroTika
    a) Za pomocą Winboxa wgrywamy plik id_rsa.pub (metoda przeciągnij i upuść)

    b) Następnie udajemy się w System > Users > SSH Keys, klikamy Import SSH Key, wybieramy użytkownika (zgodnego z tym podanym w etherspeed.conf), Jako Key file podajemy plik, który wgraliśmy i klikamy Import SSH Key.

       lub za pomocą terminala wydajemy polecenie:

       /user ssh-keys import public-key-file=id_rsa.pub user=admin


II - KONFIGURACJA:
Większość zmiennych konfiguracyjnych opisana jest w pliku etherspeed.conf. Listę urządzeń umieszczamy w devices.list zgodnie z wytycznymi z punktu 3.1

III - UŻYCIE:
Plik etherspeed-check można wykonywać cyklicznie za pomocą crontab komendą "/bin/bash /sciezka/do/etherspeed-check &> /dev/null" lub w pętli w screenie. Nalezy pamiętać, żeby ustawić odpowiednią opcję w etherspeed/conf (mode=loop wraz z crontabem spowoduje narastającą liczbę instancji etherspeed-check)

3.1 Format pliku devices.list

IP;device_name;device_type;linkspeed;port;method;reset

IP - adres IP w formacie 192.168.0.1
device_name - nazwa urządzenia wyświetlana przez skrypt
device_type - 1 dla Ubiquiti, 0 dla MikroTika, domyślnie MikroTik
linkspeed - porządana prędkość linku - 1000 lub 100 Mbps
port - port do sprawdzania, domyślnie ether1 dla Mikrotika i eth0 dla Ubiquiti
method - Metoda logowania się do urządzenia - 0 dla pubkey, 1 dla hasła, domyślnie pubkey
reset - pozwolenie na automatyczny restart urządzenia (0/1). W przypadku 0 tylko wysyła powiadomienie na e-mail administratorów
