# report-2
# 작성자 : 헝거르졸
# 관계행렬 기반 동치관계 판정 및 폐포 생성 프로그램
# 집합 A = {1,2,3,4,5}

A = [1, 2, 3, 4, 5]
N = 5

# ------------------------------
# True/False -> 참/거짓 출력용
# ------------------------------
def tf(val):
    return "참" if val else "거짓"


# ------------------------------
# 1. 관계행렬 입력
# ------------------------------
def read_matrix():
    print("5×5 관계행렬을 입력하세요 (0 또는 1):")
    R = []
    for i in range(N):
        row = list(map(int, input(f"{i+1}행: ").split()))
        if len(row) != N:
            raise ValueError("각 행에는 반드시 5개의 값이 있어야 합니다.")
        R.append(row)
    return R


# ------------------------------
# 2. 반사 / 대칭 / 추이 판정
# ------------------------------
def is_reflexive(R):
    return all(R[i][i] == 1 for i in range(N))

def is_symmetric(R):
    return all(R[i][j] == R[j][i] for i in range(N) for j in range(N))

def is_transitive(R):
    for i in range(N):
        for j in range(N):
            if R[i][j] == 1:
                for k in range(N):
                    if R[j][k] == 1 and R[i][k] == 0:
                        return False
    return True

def is_equivalence(R):
    return is_reflexive(R) and is_symmetric(R) and is_transitive(R)


# ------------------------------
# 3. 동치류 출력 기능
# ------------------------------
def equivalence_class(R, x):
    idx = A.index(x)
    eq = []
    for j in range(N):
        if R[idx][j] == 1:
            eq.append(A[j])
    return eq

def print_equivalence_classes(R):
    print("\n[동치류 출력]")
    for x in A:
        eq = equivalence_class(R, x)
        print(f"{x} 의 동치류: {eq}")


# ------------------------------
# 4. 폐포 생성 기능
# ------------------------------
def reflexive_closure(R):
    Rc = [row[:] for row in R]
    for i in range(N):
        Rc[i][i] = 1
    return Rc

def symmetric_closure(R):
    Rc = [row[:] for row in R]
    for i in range(N):
        for j in range(N):
            if Rc[i][j] == 1:
                Rc[j][i] = 1
    return Rc

def transitive_closure(R):
    Rc = [row[:] for row in R]
    for k in range(N):
        for i in range(N):
            for j in range(N):
                Rc[i][j] = Rc[i][j] or (Rc[i][k] and Rc[k][j])
    return Rc


# ------------------------------
# 행렬 출력 함수
# ------------------------------
def print_matrix(M, title="행렬"):
    print(f"\n[{title}]")
    for row in M:
        print(" ".join(map(str, row)))


# ------------------------------
# 메인 실행
# ------------------------------
def main():
    R = read_matrix()

    # 판정 결과 출력
    print("\n[관계의 성질 판정]")
    print("반사적?", tf(is_reflexive(R)))
    print("대칭적?", tf(is_symmetric(R)))
    print("추이적?", tf(is_transitive(R)))

    if is_equivalence(R):
        print("\n=> 동치 관계입니다.")
        print_equivalence_classes(R)
    else:
        print("\n=> 동치 관계가 아닙니다.")

    # 폐포 계산
    print("\n==============================")
    print("  📌 폐포(Closure) 생성 결과  ")
    print("==============================")

    # 반사 폐포
    Rc1 = reflexive_closure(R)
    print_matrix(R, "반사 폐포 변환 전")
    print_matrix(Rc1, "반사 폐포 변환 후")

    # 대칭 폐포
    Rc2 = symmetric_closure(Rc1)
    print_matrix(Rc2, "대칭 폐포 변환 후")

    # 추이 폐포
    Rc3 = transitive_closure(Rc2)
    print_matrix(Rc3, "추이 폐포 변환 후 (최종)")

    # 폐포 적용 후 동치관계 여부
    print("\n[폐포 후 동치관계 판정]")
    print("반사적?", tf(is_reflexive(Rc3)))
    print("대칭적?", tf(is_symmetric(Rc3)))
    print("추이적?", tf(is_transitive(Rc3)))

    if is_equivalence(Rc3):
        print("\n=> 폐포 적용 후 동치 관계가 됩니다.")
        print_equivalence_classes(Rc3)
    else:
        print("\n=> 폐포 적용 후에도 동치 관계가 아닙니다.") 


# ------------------------------
# 프로그램 실행
# ------------------------------
if __name__ == "__main__":
    main()
