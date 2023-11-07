from multiprocessing import Pool
import random

# Função serial de merge sort
def merge_sort_serial(lst):
    if len(lst) > 1:
        mid = len(lst) // 2
        L = merge_sort_serial(lst[:mid])
        R = merge_sort_serial(lst[mid:])

        lst = []
        while L and R:
            if L[0] < R[0]:
                lst.append(L.pop(0))
            else:
                lst.append(R.pop(0))
        lst.extend(L if L else R)
    return lst

# Função de merge usada na paralelização
def merge(left, right):
    result = []
    while left and right:
        if left[0] < right[0]:
            result.append(left.pop(0))
        else:
            result.append(right.pop(0))
    result.extend(left if left else right)
    return result

# Função paralela de merge sort
def merge_sort_parallel(array):
    if len(array) <= 1:
        return array
    else:
        mid = len(array) // 2
        with Pool(2) as p:
            left, right = p.map(merge_sort_parallel, [array[:mid], array[mid:]])
        return merge(left, right)

# Gerando uma lista de números aleatórios
random_list = [random.randint(0, 1000) for _ in range(2000)]  # Lista com 2000 inteiros aleatórios

# Medindo o tempo da versão serial
start_time_serial = time.time()
sorted_serial = merge_sort_serial(random_list.copy())
end_time_serial = time.time()

# Medindo o tempo da versão paralela
start_time_parallel = time.time()
sorted_parallel = merge_sort_parallel(random_list.copy())
end_time_parallel = time.time()

# Tempos de execução
time_serial = end_time_serial - start_time_serial
time_parallel = end_time_parallel - start_time_parallel

time_serial, time_parallel

