#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <string.h>

#define ALFABE_UZUNLUGU 26

// Trie düğüm yapısı
typedef struct TrieNode {
    struct TrieNode* cocuklar[ALFABE_UZUNLUGU];
    bool kelimeSonu;
} TrieNode;

// Yeni bir Trie düğümü oluşturur
TrieNode* yeniDugum() {
    TrieNode* dugum = (TrieNode*)malloc(sizeof(TrieNode));
    if (dugum) {
        dugum->kelimeSonu = false;
        for (int i = 0; i < ALFABE_UZUNLUGU; i++) {
            dugum->cocuklar[i] = NULL;
        }
    }
    return dugum;
}

// Kelimeyi Trie'ye ekler
void ekle(TrieNode* kok, const char* kelime) {
    TrieNode* gecici = kok;
    int uzunluk = strlen(kelime);
    for (int seviye = 0; seviye < uzunluk; seviye++) {
        int indeks = kelime[seviye] - 'a';
        if (!gecici->cocuklar[indeks]) {
            gecici->cocuklar[indeks] = yeniDugum();
        }
        gecici = gecici->cocuklar[indeks];
    }
    gecici->kelimeSonu = true;
}

// Kelimenin Trie'de olup olmadığını kontrol eder
bool ara(TrieNode* kok, const char* kelime) {
    TrieNode* gecici = kok;
    int uzunluk = strlen(kelime);
    for (int seviye = 0; seviye < uzunluk; seviye++) {
        int indeks = kelime[seviye] - 'a';
        if (!gecici->cocuklar[indeks]) {
            return false;
        }
        gecici = gecici->cocuklar[indeks];
    }
    return (gecici != NULL && gecici->kelimeSonu);
}

int main() {
    char kelimeler[][8] = { "elma", "karpuz", "muz", "üzüm", "armut" };
    int n = sizeof(kelimeler) / sizeof(kelimeler[0]);

    TrieNode* kok = yeniDugum();

    // Trie'ye kelimeleri ekle
    for (int i = 0; i < n; i++) {
        ekle(kok, kelimeler[i]);
    }

    // Kelimeleri ara ve sonuçları yazdır
    printf("Kelime Arama Sonuclari:\n");
    printf("elma: %s\n", ara(kok, "elma") ? "Var" : "Yok");
    printf("karpuz: %s\n", ara(kok, "karpuz") ? "Var" : "Yok");
    printf("muz: %s\n", ara(kok, "muz") ? "Var" : "Yok");

    return 0;
}
