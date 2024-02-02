package main

import (
	. "fmt"
)

func CreaTabella(n int) [][]float64 { //funzione che crea la tabella di Cantor
	var tabella [][]float64
	for i := 1; i < n; i++ {
		var rigapositiva []float64
		for j := 0; j < n; j++ {
			rigapositiva = append(rigapositiva, (float64(j) / float64(i))) //aggiungiamo la riga con i rapporti n/m
		}
		var riganegativa []float64
		for j := 0; j < n; j++ {
			riganegativa = append(riganegativa, (-float64(j) / float64(i)))
		}
		tabella = append(tabella, rigapositiva, riganegativa) //aggiungiamo la riga con i rapporti -n/m
	}
	return tabella
}

func PercorriTabella(n int, tabella [][]float64) []float64 { //percorriamo la tabella in senso diagonale da sx a dx dal basso verso l'alto
	var razionali []float64
	for i := 0; i < n; i++ {
		for j := 0; j < i+1; j++ {
			k := 0
			if i != 0 && j != 0 {
				n := tabella[i-j-k][j]
				razionali = append(razionali, n)
				k++
			} else if j == 0 {
				n := tabella[i][j]
				razionali = append(razionali, n)
			}
		}

	}
	return razionali //resituiamo la tabella con tutti i razionali trovati nel percorso
}

func CreaLista(razionali []float64) []float64 { //funzione che crea una lista con i razionali presenti una sola volta
	var unici []float64
	for i, v := range razionali {
		if Unici(i, v, razionali) {
			unici = append(unici, v)
		}
	}
	return unici
}

func Unici(i int, n float64, razionali []float64) bool { //funzione che controlla se un numero razionale è già stato trovato in precedenza
	for j := 0; j < i; j++ {
		if n == razionali[j] {
			return false
		}
	}
	return true
}

func StampaOutput(lista []float64, n int) { //funzione che stampa la lista dei numeri naturali con una biezione da N a Q fino alla soglia stabilita
	for i := 0; i <= n; i++ {
		Printf("f(%d) = %g;\n", i, lista[i])
	}
}

func main() {
	var n int
	Print("\nBenvenuto al programma sulla numerabilità dell'insieme dei razionali Q (di GIUGNI GABRIELE, matr. 31394A)\nInserisci la soglia desiderata: ")
	Scan(&n)
	Println("\nI numeri razionali numerati attraverso una biezione N -> Q fino alla soglia stabilita sono:")
	tab := CreaTabella(n)
	StampaOutput(CreaLista(PercorriTabella(n, tab)), n)
}
