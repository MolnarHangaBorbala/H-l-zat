1.Lépés: Szerverbe modul berakása
    **PT-HOST-NM-1CGE**
    | Helyzet                                | Modul javaslat                                 |
    | -------------------------------------- | ---------------------------------------------- |
    | Server0 – másik eszköz is FastEthernet | `PT-HOST-NM-1CFE`                              |
    | Server0 – másik eszköz GigabitEthernet | `PT-HOST-NM-1GE`                               |
    | Nem tudod biztosan?                    | `PT-HOST-NM-1CFE` (biztonságos alapértelmezés) |


2.Lépés: Összekötés
    **1. PC0 ↔ Router0**
      Kábel: Copper Straight-Through
      Portok:
        PC0: FastEthernet0
        Router0: FastEthernet0/0

    **2. Router0 ↔ Cluster0 (ISP)**
      Kábel: Copper Straight-Through
      Portok:
        Router0: FastEthernet0/1
        Cluster0: Ethernet port (pl. FastEthernet1)

    **3. Cluster0 ↔ Server1**
      Kábel: Copper Straight-Through
      Portok:
        Cluster0: Ethernet port (pl. GigaEthernet)
        Server1: GigaEthernet0

3.Lépés: IP-címek beállítása
    

💡 Ha a Server1-be új portot tettél (pl. PT-HOST-NM-1CFE), használd azt: pl. FastEthernet1




-------------------------------------------------------------------------------------------------------------------------------------------------------------

Nézzük lépésről lépésre, hogyan kell kábelezni a **PC0**, **Router0**, **Server0**, és **Server1** eszközöket.

### **Hálózati eszközök és portok:**

* **PC0 és Router0** közötti kapcsolat: A PC0 és a Router0 közvetlenül csatlakoznak, a **PC0** az **Fa0** porton keresztül, míg a **Router0** **Gig0/0** portját használja.

* **Router0 és Cluster (Server0)** közötti kapcsolat: A **Router0** **Gig0/1** portja csatlakozik a **Cluster**-hez, amely tartalmazhat **Server0**-t, **ISP**-t és **DHCP**-t.

* **Router0 és Server1** közötti kapcsolat: A **Router0** **Gig0/1** portja közvetlenül csatlakozik **Server1**-hez.

### **Kábelezés és portok:**

#### 1. **PC0 és Router0 közötti kapcsolat:**

* **Kábel típusa**: **Cross-over kábel**.
* **PC0**: **Fa0 port**.
* **Router0**: **GigabitEthernet 0/0 port**.

**Kábelezés lépései**:

* Válassz egy **Cross-over kábel**-t.
* Csatlakoztasd a **PC0 Fa0** portját a **Router0 Gig0/0** portjához.

#### 2. **Router0 és Cluster (Server0) közötti kapcsolat:**

* **Kábel típusa**: **Straight-through kábel**.
* **Router0**: **GigabitEthernet 0/1 port**.
* **Server0 (Cluster részeként)**: Csatlakoztasd a **Server0**-t, amely közvetlenül a **Router0**-hoz kapcsolódik.

**Kábelezés lépései**:

* Válassz egy **Straight-through kábel**-t.
* Csatlakoztasd a **Router0 Gig0/1** portját a **Server0**-hoz (cluster port).

#### 3. **Router0 és Server1 közötti kapcsolat:**

* **Kábel típusa**: **Straight-through kábel**.
* **Router0**: **GigabitEthernet 0/1 port**.
* **Server1**: **Fa1 port** (Server1).

**Kábelezés lépései**:

* Válassz egy **Straight-through kábel**-t.
* Csatlakoztasd a **Router0 Gig0/1** portját a **Server1 Fa1** portjához.

---

### **IP címek konfigurálása**

#### **PC0**:

* **IP cím**: **192.168.2.2**
* **Subnet mask**: **255.255.255.240** (/28)
* **Default Gateway**: **192.168.2.1** (Router0 Gig0/0 port)

#### **Router0**:

* **Gig0/0 (PC0 oldal)**:

  * **IP cím**: **192.168.2.1**
  * **Subnet mask**: **255.255.255.240** (/28)

* **Gig0/1 (Server0 és Server1 oldal)**:

  * **IP cím**: **192.168.1.1**
  * **Subnet mask**: **255.255.255.252** (/30)

#### **Server0 (Cluster részeként)**:

* **IP cím**: **192.168.1.2**
* **Subnet mask**: **255.255.255.252** (/30)
* **Default Gateway**: **192.168.1.1** (Router0 Gig0/1)

#### **Server1**:

* **IP cím**: **192.168.1.3**
* **Subnet mask**: **255.255.255.252** (/30)
* **Default Gateway**: **192.168.1.1** (Router0 Gig0/1)

---

### **Összefoglalás:**

* **PC0** → **Router0 Gig0/0**: **Cross-over kábel** (PC0 és Router0 közvetlen kapcsolat).
* **Router0 Gig0/1** → **Cluster (Server0)**: **Straight-through kábel** (Router0 és Server0 közvetlen kapcsolat).
* **Router0 Gig0/1** → **Server1 Fa1**: **Straight-through kábel** (Router0 és Server1 közvetlen kapcsolat).

---

### **Miért választunk más-más kábeleket?**

* **Cross-over kábel**: A **PC0** és **Router0** közvetlen kapcsolatakor, mivel mindkettő hálózati eszköz.
* **Straight-through kábel**: A **Router0** és a **Szerverek** közötti kapcsolatban, mert a router és a szerverek különböző típusú eszközök.

---

### **Tesztelés:**

1. **Ping teszt (PC0)**:

   * A **PC0**-ról pingeld meg a **Router0**-t: `ping 192.168.2.1`
   * Ez ellenőrzi, hogy a **PC0** sikeresen kommunikál-e a **Router0** **Gig0/0** portjával.

2. **Ping teszt (Server0 és Server1)**:

   * A **PC0**-ról pingeld meg **Server1**-et: `ping 192.168.1.3`
   * Ez ellenőrzi, hogy a **Router0** helyesen irányítja-e a forgalmat a **Server1**-hez.

---
