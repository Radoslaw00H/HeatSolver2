Projekt: Spektralny solver równania Poissona 2D (High-Level Plan)

Cel projektu:
Rozwiązać równanie Poissona w 2D:

Laplacian(phi(x,y)) = f(x,y)

- Typowe w symulacjach elektrycznych, potencjału grawitacyjnego, przepływów cieczy
- W pełni matematyczny, idealny do HPC w Fortranie

1. High-Level Logic

1.1 Definicja problemu
- Siatka Nx × Ny w przestrzeni 2D
- Funkcja źródłowa f(x,y) (np. punktowy ładunek lub gęstość masy)
- Warunki brzegowe (Dirichlet lub Neumann)

1.2 Transformacja do dziedziny częstotliwości
- Zastosowanie FFT 2D (Fast Fourier Transform)
- W dziedzinie częstotliwości równanie Poissona redukuje się do prostego dzielenia:

  phi_hat(kx,ky) = f_hat(kx,ky) / (- kx^2 - ky^2)

1.3 Transformacja wstecz
- Odwrócone FFT daje rozwiązanie phi(x,y) w przestrzeni

1.4 Walidacja i zapis wyników
- Porównanie z rozwiązaniem analitycznym (jeśli dostępne)
- Eksport do CSV / możliwość wizualizacji

2. Matematyka / Obliczenia

- Macierz Nx × Ny: każda wartość = punkt w przestrzeni
- FFT 2D: przekształcenie całej macierzy do dziedziny częstotliwości
- Operacje element-wise: dzielenie przez (- kx^2 - ky^2)
- Inverse FFT: powrót do przestrzeni
- Opcjonalne: normowanie, skalowanie, handling boundary

3. Funkcje / Moduły (High-Level)

- init_grid(nx, ny)
  Tworzy siatkę 2D i ustawia f(x,y)

- fft2d(f)
  Przelicza funkcję źródłową do dziedziny częstotliwości

- solve_poisson(f_hat)
  Liczy phi_hat(kx,ky) = f_hat / (-kx^2 - ky^2)

- ifft2d(phi_hat)
  Odwraca FFT → phi(x,y)

- validate(phi)
  Porównuje wynik z rozwiązaniem analitycznym lub sprawdza normy błędu

- export_csv(phi)
  Zapisuje wyniki w pliku

4. XX

- Wymaga pracy na dużych tablicach → Fortran idealny
- Możliwość równoległości (OpenMP, vectorization)
- Matematyka = czysta → portfolio widać, że rozumiesz PDE i FFT
- Możliwość rozbudowy: różne boundary conditions, funkcje źródłowe, 3D

5. XX

- Benchmarking: czas rozwiązania vs rozmiar siatki
- Porównanie CPU vs optymalizacja Fortran compiler flags
- Heatmapa 2D (w Pythonie, opcjonalnie)
- Modularność: każdy moduł osobno → B2B style

6. Podsumowanie High-Level

1. Zainicjalizuj siatkę i f(x,y)
2. FFT 2D → f_hat
3. Podziel w dziedzinie częstotliwości → phi_hat = f_hat / (-kx^2 - ky^2)
4. Inverse FFT → phi(x,y)
5. Walidacja + zapis wyników







HeatSolver2D/           <-- główny folder projektu
│
├─ src/                 <-- folder z kodem źródłowym
│   ├─ main.f90         <-- program główny, high-level flow
│   ├─ solver.f90       <-- moduł obliczeniowy: step, PDE solver
│   └─ utils.f90        <-- moduł pomocniczy: inicjalizacja, I/O, eksport CSV
│
├─ output/              <-- folder na pliki wynikowe (CSV, wizualizacje)
│   └─ (tu będą pliki wynikowe np. heat_step_50.csv)
│
├─ README.md            <-- opis projektu, high-level logic, instrukcje
│
└─ LICENSE              <-- opcjonalnie: licencja open-source

