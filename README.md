-- ModuleScript: MonsterDatabase
-- Coloque em ReplicatedStorage ou ServerScriptService

local MonsterDatabase = {}

-- Lista de monstros e as evidências necessárias (nomes de evidência são strings).
-- Você pode editar/expandir essa tabela.
-- A lógica usa conjunto: se todas as evidências do conjunto estiverem presentes -> corresponde ao monstro.
MonsterDatabase.Monsters = {
    {
        Name = "Spectral Wraith",
        RequiredEvidence = {"Orbe", "Whisper", "ColdSpot"} -- exemplo
    },
    {
        Name = "Shadow Stalker",
        RequiredEvidence = {"Footprints", "EMF5", "ShadowSight"}
    },
    {
        Name = "Banshee",
        RequiredEvidence = {"Scream", "EMF4", "Writing"}
    },
    {
        Name = "Poltergeist",
        RequiredEvidence = {"ObjectMovement", "EMF5", "Targets"}
    },
    {
        Name = "Wisp",
        RequiredEvidence = {"Orbe", "Noises"}
    },
    -- Adicione mais monstros conforme quiser...
}

-- Função utilitária: verifica se setA (table como keys=true) contém todos elementos de listB
local function containsAll(setA, listB)
    for _, v in ipairs(listB) do
        if not setA[v] then
            return false
        end
    end
    return true
end

-- Identifica monstro a partir de uma lista de evidências coletadas
-- evidences: array of strings
-- retorna: monsterName ou nil se não identificado unicamente
function MonsterDatabase.identify(evidences)
    -- construir set para buscas rápidas
    local evidSet = {}
    for _, e in ipairs(evidences) do evidSet[e] = true end

    local matches = {}

    for _, monster in ipairs(MonsterDatabase.Monsters) do
        if containsAll(evidSet, monster.RequiredEvidence) then
            table.insert(matches, monster.Name)
        end
    end

    if #matches == 1 then
        return matches[1] -- identificado com certeza
    elseif #matches > 1 then
        -- múltiplas opções — devolve as possíveis (você pode mudar a política)
        return matches
    else
        return nil
    end
end

return MonsterDatabase
