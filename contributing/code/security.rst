Problemi di sicurezza
=====================

Questo documento spiega la gestione da parte della squadra di Symfony  dei problemi di sicurezza
di Symfony (in cui "Symfony" è il codice ospitato nel `repository Git`_ ``symfony/symfony``).


Segnalare un problema di sicurezza
----------------------------------

Se si è trovato un problema di sicurezza in Symfony, non utilizzare la
lista o il bug tracker e non diffonderlo pubblicamente. Tutte le questioni di
sicurezza devono essere inviate a **security [at] symfony-project.com**. Le email
inviate a questo indirizzo verranno inoltrate alla squadra di sviluppo di Symfony .

Processo di risoluzione
-----------------------

Per ogni rapporto, prima si cercherà di confermare la vulnerabilità. Quando
confermata, la squadra di sviluppo lavorerà a una soluzione seguendo questi passi:

1. Inviare un riconoscimento al segnalatore;
2. Lavorare su una patch;
3. Ottenere un identificatore CVE da mitre.org;
4. Scrivere un annuncio sul `blog`_ di Symfony, che descriva la vulnerabilità.
   Tale post dovrebbe contenere le seguenti informazioni:

   * un titolo che includa sempre la stringa "Security release";
   * una descrizione della vulnerabilità;
   * le versioni afflitte;
   * i possibili exploit;
   * come applicare patch/aggiornamenti/workaround alle applicazioni afflitte;
   * l'identificatore CVE;
   * riconoscimenti.
5. Inviare patch e annuncio al segnalante per una revisione;
6. Applicare la patch a tutte le versioni di Symfony in manutenzione;
7. Pacchettizzare nuove versioni per tutte le versioni afflitte;
8. Pubblicare il post sul `blog`_ ufficiale di Symfony (va anche aggiunti alla
   categoria "`Security Advisories`_");
9. Aggiornare la lista degli avvisi di sicurezza (vedere sotto).

.. note::

    I rilasci che includono questioni di sicurezza non andrebbero fatti di sabato o
    domenica, a meno che la vulnerabilità non sia stata resa pubblica.

.. note::

   Mentre la patch è in corso di lavorazione, si prega di non rivelare pubblicamente la problematica.

.. note::

    La risoluzione può prendere tra un paio di giorni a un mese, a seconda
    della complessità e del coordinamento tra i progetti a valle (vedere
    il paragrafo successivo).

Collaborazione con progetti open source a valle
-----------------------------------------------

Poiché Symfony è usato da molti progetti open source, il modo in cui la
squadra di sicurezza di Symfony collabora sulle problematiche di sicurezza è stata standardizzata
con i progetti a valle. Il progetto funziona come segue:

1. Dopo che la squadra di sicurezza di Symfony ha riconosciuto la problematica, invia
immediatamente una email alle squadre di sicurezza dei progetti a valle, per informarli
della probelamtica;

2. La squadra di sicurezza di Symfony crea un repository Git privato, per facilitare la
collaborazione sulla problematica. L'accesso a tale repository è fornito all
squadra di sicurezza di Symfony, ai contributori du Symfony che hanno avuto impatto sulla
problematica e a un rappresentante i ogni progetto a valle;

3. Le persone che accedono al repository privato lavorano a una soluzione per
risolvere la problematica, tramire richieste di pull, revisioni di codice e commenti;

4. Una volta trovata la soluzione, tutti i progetti coinvolti collaborano per trovare
la data migliore per un rilascio congiunto (non c'è garanzia che tutti i rllasci saranno
contempoaranei, ma si tenterà il più possibili di pubblicarli nello stesso periodo). Quando
non si ritiene che la problematica abbia subito degli exploit, un periodo di due settimane
sembra essere ragionevole.

La lista dei progetti a valle partecipanti a tale processo è manutenuta più corta
possibile, per meglio gestire il flusso di informazioni riservate, prima
della pubblicazione. Per questo motivo, i progetti saranno inclusi a sola discrezione
della squadra di sicurezza di Symfony.

A oggi, i seguenti progetti hanno approvato questo processo e sono parte dei
progetti a valle inclusi:

* Drupal (solitamente con rilasci di venerdì)
* eZPublish

Bollettini di sicurezza
-----------------------

Questa sezione elenca le vulnerabilità di sicurezza che sono state risolte in Symfony,
partendo da Symfony 1.0.0:

* 17 gennaio 2013: `Security release: Symfony 2.0.22 and 2.1.7 released <http://symfony.com/blog/security-release-symfony-2-0-22-and-2-1-7-released>`_ (`CVE-2013-1348 <http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1348>`_ and `CVE-2013-1397 <http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-1397>`_)
* 20 dicembre 2012: `Security release: Symfony 2.0.20 and 2.1.5 <http://symfony.com/blog/security-release-symfony-2-0-20-and-2-1-5-released>`_  (`CVE-2012-6431 <http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-6431>`_ and `CVE-2012-6432 <http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-6432>`_)
* 29 novembre 2012: `Security release: Symfony 2.0.19 and 2.1.4 <http://symfony.com/blog/security-release-symfony-2-0-19-and-2-1-4>`_
* 25 novembre 2012: `Security release: symfony 1.4.20 released  <http://symfony.com/blog/security-release-symfony-1-4-20-released>`_
* 28 agosto 2012: `Security Release: Symfony 2.0.17 released <http://symfony.com/blog/security-release-symfony-2-0-17-released>`_
* 30 maggio 2012: `Security Release: symfony 1.4.18 released <http://symfony.com/blog/security-release-symfony-1-4-18-released>`_
* 24 febbraio 2012: `Security Release: Symfony 2.0.11 released <http://symfony.com/blog/security-release-symfony-2-0-11-released>`_
* 16 novembre 2011: `Security Release: Symfony 2.0.6 <http://symfony.com/blog/security-release-symfony-2-0-6>`_
* 21 marzo 2011: `symfony 1.3.10 and 1.4.10: security releases <http://symfony.com/blog/symfony-1-3-10-and-1-4-10-security-releases>`_
* 29i giugno 2010: `Security Release: symfony 1.3.6 and 1.4.6 <http://symfony.com/blog/security-release-symfony-1-3-6-and-1-4-6>`_
* 31 maggio 2010: `symfony 1.3.5 and 1.4.5 <http://symfony.com/blog/symfony-1-3-5-and-1-4-5>`_
* 25 febbraio 2010: `Security Release: 1.2.12, 1.3.3 and 1.4.3 <http://symfony.com/blog/security-release-1-2-12-1-3-3-and-1-4-3>`_
* 13 febbraio, 2010: `symfony 1.3.2 and 1.4.2 <http://symfony.com/blog/symfony-1-3-2-and-1-4-2>`_
* 27 aprile 2009: `symfony 1.2.6: Security fix <http://symfony.com/blog/symfony-1-2-6-security-fix>`_
* 3 ottobre 2008: `symfony 1.1.4 released: Security fix <http://symfony.com/blog/symfony-1-1-4-released-security-fix>`_
* 14 maggio 2008: `symfony 1.0.16 is out  <http://symfony.com/blog/symfony-1-0-16-is-out>`_
* 1 aprile 2008: `symfony 1.0.13 is out  <http://symfony.com/blog/symfony-1-0-13-is-out>`_
* 21 marzo 2008: `symfony 1.0.12 is (finally) out ! <http://symfony.com/blog/symfony-1-0-12-is-finally-out>`_
* 25 giugno 2007: `symfony 1.0.5 released (security fix) <http://symfony.com/blog/symfony-1-0-5-released-security-fix>`_

.. _repository Git:      https://github.com/symfony/symfony
.. _blog:                http://symfony.com/blog/
.. _Security Advisories: http://symfony.com/blog/category/security-advisories
