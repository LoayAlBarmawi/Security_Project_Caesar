import time
from collections import Counter


def analyze(ciphertext):
    list1 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',
             'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
             'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    guess = Counter(ciphertext)
    guess = max(guess, key=guess.get)
    key=0
    if list1.index(guess)>list1.index('E'):
        key = list1.index(guess)-list1.index('E')
    else:
        key = list1.index('E')-list1.index(guess)
    print("Ciphertext : " + ciphertext)
    print("Key : " + str(key))
    print("Plaintext: " + decrypt(ciphertext, key))


def encrypt(plaintext, key):
    ciphertext = ""
    list1 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',
             'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
             'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    for i in range(len(plaintext)):
        letter = plaintext[i]
        if ord(letter) == 32:
            continue
        if letter.isalpha():
            index=list1.index(letter.upper())
            ciphertext += list1[(index+key) % 62]
        if letter.isnumeric():
            index=list1.index(letter)
            ciphertext += list1[(index+key) % 62]

    return ciphertext


def decrypt(ciphertext, key):
    list1 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',
             'U', 'V', 'W', 'X', 'Y', 'Z','a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
             'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z','0','1','2','3','4','5','6','7','8','9']
    plaintext = ""
    for i in range(len(ciphertext)):
        letter = ciphertext[i]
        index = list1.index(letter)
        plaintext += list1[(index-key) % 62]
    return plaintext


def bruteforce(ciphertext):
    t0 = time.perf_counter()
    list1 = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T',
             'U', 'V', 'W', 'X', 'Y', 'Z', 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
             'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
    for j in range(1, 62):
        plaintext = ""
        for i in range(len(ciphertext)):
            letter = ciphertext[i]
            index = list1.index(letter)
            plaintext += list1[(index - j) % 62]
        print("key: " + str(j))
        print("result: " + plaintext)
    t1 = time.perf_counter()
    print("time to crack: ", t1 - t0)


plain = input("enter plaintext")
key = int(input('enter key'))

print("Plaintext : " + plain)
print("Key : " + str(key))
print("Ciphertext: " + encrypt(plain, key))

cipher = input("enter ciphertext")
key = int(input('enter key'))

print("Ciphertext : " + cipher)
print("Key : " + str(key))
print("Plaintext: " + decrypt(cipher, key))

cipher = input("enter ciphertext")
print("brute force attempts:\n")
bruteforce(cipher)
analyze(cipher)