Chương 2: Sắp xếp: 
#include<stdio.h>
#define MAX_N 100
typedef int keytype;
typedef float othertype;
typedef struct {
	keytype key;
	othertype other;
}record;
void swap(record *a, record *b){
	record temp;
	temp = *a;
	*a = *b;
	*b = temp;
}
void selectionSort(record a[], int n){
	for (int i = 0; i < n-1; i++){
		keytype lowkey = a[i].key;
		int lowidx = i;
		for (int j = i+1; j < n; j++){
			if (a[j].key < lowkey){
				lowkey = a[j].key;
				lowidx = j;
			}
		}
		swap(&a[i],&a[lowidx]);
	}
}

void readFile (record a[], int *n){    
	FILE *f = fopen("data.txt","r");
	if(f!=NULL)
        while (!feof(f)){
            fscanf(f,"%d%f",&a[*n].key , &a[*n].other);
            (*n)++;
        }
    else printf("loi moi file\n ");
	fclose(f);
}

void printData(record a[], int n){
	for(int i=0; i < n; i++)
		printf("%3d %5d %8.2f\n", i+1, a[i].key, a[i].other);
}

int main (){
	record a[MAX_N];
	int n = 0;
	printf("Thuat toan Selection Sort\n\n");
	readFile(a, &n);
	printf("Du lieu truoc khi sap xep:\n");
	printData(a, n);
	selectionSort(a, n);
	printf("Du lieu sau khi sap xep:\n");
	printData(a, n);
	return 0;
}
*Thế vào chỗ tô đỏ:
- Sắp xếp chọn (Selection Sort):
void selectionSort(record a[], int n){
	for (int i = 0; i < n-1; i++){
		keytype lowkey = a[i].key;
		int lowidx = i;
		for (int j = i+1; j < n; j++){
			if (a[j].key < lowkey){
				lowkey = a[j].key;
				lowidx = j;
			}
		}
		swap(&a[i],&a[lowidx]);
	}
}
- Sắp xếp xen(Insertion Sort):
void insertionSort(record a[], int n){
	for (int i = 1; i < n; i++){
		int j = i;
		while((j > 0) && (a[j].key < a[j-1].key)){
			swap(&a[j], &a[j-1]);
			j--;
		}
	}
}
- Sắp xếp nổi bọt (Bubble Sort):
void bubbleSort(record a[], int n){
	for(int i = 0; i < n-1; i++)
		for(int j = n-1; j > i; j--)
			if(a[j].key < a[i].key)
				swap(&a[i], &a[j]);
}
- Sắp xếp vun đống (Heap Sort):
void heapSort(record a[], int n){
	int i;
	for(i = (n-2)/2; i >= 0; i--)
		pushDown(a, i, n-1);
	for(i = n-1; i >= 2; i--){
		swap(&a[0], &a[i]);
		pushDown(a, 0, i-1);
	}
	swap(&a[0], &a[1]);
}

Chương 3: Tham ăn: 
Cái balo:
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int w, v, qty;
    float ppu; //price per unit
    char name[25];    
} item;

void swap (item *a, item* b){
    item temp = *a;
    *a = *b;
    *b = temp;
}

void sort (item *a, int n){
    for(int i = 0; i < n-1; i++)
        for(int j = i+1; j < n; j++)
            if(a[i].ppu < a[j].ppu)
                swap(&a[i], &a[j]);
}

void readFile (item **a, int *n, int *m){    
    FILE *f = fopen("CaiBalo1.txt", "r");
    *a = (item*)malloc(sizeof(item));
    fscanf(f, "%d", m);
    int i = 0;    
    while (!feof(f)){
        (*a) = realloc(*a, sizeof(item)*(i+1));
        fscanf(f, "\n%d %d %[^\r\n]s", &(*a)[i].w, &(*a)[i].v, (*a)[i].name);
        (*a)[i].ppu = (float)(*a)[i].v / (*a)[i].w;
        (*a)[i].qty = 0;
        i++;
    }
    *n = i;
    fclose(f);
}

void printChart (item *a, int n){
    int total_val = 0, total_weight = 0;
    printf("|---|---------------------|-----------|-------|-------|---------|\n");
	printf("|%-3s|%-21s|%-11s|%-7s|%-7s|%-9s|\n", "STT", "     Ten do vat", "Trong luong", "Gia tri", "Don gia","Phuong an");
	printf("|---|---------------------|-----------|-------|-------|---------|\n");
	for(int i = 0, k = 1; i < n; i++){        
        printf("| %-2d| %-20s|%11d|%7d|%7.2f|%9d|\n", k++, a[i].name, a[i].w, a[i].v, a[i].ppu, a[i].qty);
        total_val += a[i].v*a[i].qty;
        total_weight += a[i].w*a[i].qty;
	}	
	printf("|---|---------------------|-----------|-------|-------|---------|\n");	
	printf("Phuong an (theo thu tu DG giam dan): X(");
	for(int i=0; i<n-1; i++){
		printf("%d,", a[i].qty);
	}	
	printf("%d)\n", a[n-1].qty);
    printf("Tong trong luong = %5d\n", total_weight);
    printf("Tong gia tri     = %5d\n", total_val);
}

void greedy(item *a, int n, int m){
    for(int i = 0; i < n; i++){
        a[i].qty = m / a[i].w;
        m -= a[i].w*a[i].qty;
    }
}

int main (){
    int n = 0, m = 0;
    item *a;
    readFile(&a, &n, &m);
    sort(a, n);
    greedy(a, n, m);
    printChart(a, n);
    free(a);
}



