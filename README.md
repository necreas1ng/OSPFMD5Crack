# OSPFMD5Crack

How to crack on OSPF MD5 hash manually

**The author has nothing to do with those who will use this tool for personal purposes to destroy other people's computer networks. The tools are presented for training purposes to help engineers improve the security of their network.**

**ᛝ**

## 1. Traffic dump

You can open **Wireshark** and capture the traffic. Save it to a **pcapng** file.

![](wireshark_dump_one.png)

## 2. Extracting hashes using Ettercap

Using the **Ettercap**  utility, we will be able to extract net-MD5 hashes from the traffic dump. **OSPF uses net-MD5 hash.** If you copy the hash value from the header **Authentication Crypt Data** and try to crack the password using the brute-force method, nothing will work.

![](raw_md5hash_from_dump.png)

![](fail_rawmd5crack.png)

Let's run **Ettercap** in console mode and feed it a traffic dump (a file with the extension **.pcapng**)

```
ettercap -Tqr ospf_md5_dump.pcapng
```

![](ospf_extract_hashes.png)

Using the **cut**, we will process the file with the hashes that **Ettercap** gave us.

```
cat raw-hashes.txt | cut -d ":" -f 2 >> net-md5-hashes.txt
```

![](netmd5-hashes.png)

## 3. Cracking net-MD5 hashes with John

Using **John**, we pass a file with hashes and a dictionary to the input. **(brute-force attack)**

```
john net-md5-hashes.txt --wordlist=brute.txt
```

![](cracked_pass.png)

Let's check the validity of the password and try to connect to the OSPF domain. Using **FRRouting.** **Cracked password - c1$c0**

![](frr.png)

We were able to crack the password from the OSPF domain and connect to the routing domain in **area 0**
