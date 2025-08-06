```

-- Tabela de Alunos
CREATE TABLE students (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    phone_number TEXT UNIQUE NOT NULL,
    level INT CHECK (level BETWEEN 1 AND 3), -- 1: Pupilo, 2: Curioso, 3: Entendido
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabela de Perguntas
CREATE TABLE questions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    question_text TEXT NOT NULL,
    options JSONB NOT NULL, -- {"a": "op1", "b": "op2", "c": "op3", "d": "op4"}
    correct_answer TEXT NOT NULL,
    difficulty INT CHECK (difficulty BETWEEN 1 AND 3),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabela de Respostas
CREATE TABLE answers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES students(id) ON DELETE CASCADE,
    question_id UUID REFERENCES questions(id) ON DELETE CASCADE,
    answer_text TEXT NOT NULL,
    is_correct BOOLEAN NOT NULL,
    score INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Tabela de Notas Semanais
CREATE TABLE scores (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id UUID REFERENCES students(id) ON DELETE CASCADE,
    week_start DATE NOT NULL,
    week_end DATE NOT NULL,
    total_score INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT NOW()
);

-- Índices para performance
CREATE INDEX idx_answers_student_id ON answers(student_id);
CREATE INDEX idx_scores_student_id ON scores(student_id);

```
