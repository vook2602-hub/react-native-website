import React, { useState } from "react"; import { SafeAreaView, View, Text, TextInput, TouchableOpacity, Switch, FlatList, StyleSheet } from "react-native";

export default function App() { const [type, setType] = useState("bottom"); // bottom = قاع, top = قمة const [pivot, setPivot] = useState(""); const [levels, setLevels] = useState("1"); const [scalp, setScalp] = useState(false); const [results, setResults] = useState([]);

const formatNumber = (n) => { return new Intl.NumberFormat("ar-IQ", { maximumFractionDigits: 2 }).format(n); };

const calculate = () => { const pivotNum = parseFloat(pivot); const lvl = parseInt(levels); if (isNaN(pivotNum) || isNaN(lvl)) return;

const stepPct = scalp ? 0.0025 : 0.005;
const step = pivotNum * stepPct;
let rows = [];

for (let i = 1; i <= lvl; i++) {
  const price = type === "bottom" ? pivotNum + step * i : pivotNum - step * i;
  rows.push({ id: i.toString(), level: i, price: formatNumber(price) });
}
setResults(rows);

};

return ( <SafeAreaView style={styles.container}> <Text style={styles.title}>HUSSEIN_ROYAL</Text> <Text style={styles.subtitle}>@Hussein_Royal</Text>

<View style={styles.card}>
    <Text style={styles.label}>اختر النوع:</Text>
    <View style={styles.row}>
      <TouchableOpacity
        style={[styles.option, type === "bottom" && styles.activeOption]}
        onPress={() => setType("bottom")}
      >
        <Text style={styles.optionText}>قاع</Text>
      </TouchableOpacity>
      <TouchableOpacity
        style={[styles.option, type === "top" && styles.activeOption]}
        onPress={() => setType("top")}
      >
        <Text style={styles.optionText}>قمة</Text>
      </TouchableOpacity>
    </View>

    <Text style={styles.label}>أدخل رقم القمة أو القاع:</Text>
    <TextInput
      style={styles.input}
      keyboardType="numeric"
      placeholder="مثال: 3644"
      value={pivot}
      onChangeText={setPivot}
    />

    <Text style={styles.label}>عدد المستويات:</Text>
    <TextInput
      style={styles.input}
      keyboardType="numeric"
      value={levels}
      onChangeText={setLevels}
    />

    <View style={styles.switchRow}>
      <Switch value={scalp} onValueChange={setScalp} />
      <Text style={styles.switchLabel}>⚡ تفعيل معادلة سكالب</Text>
    </View>

    <TouchableOpacity style={styles.button} onPress={calculate}>
      <Text style={styles.buttonText}>احسب النتائج</Text>
    </TouchableOpacity>

    <View style={styles.results}>
      {results.length === 0 ? (
        <Text style={styles.muted}>📊 النتائج ستظهر هنا</Text>
      ) : (
        <FlatList
          data={results}
          keyExtractor={(item) => item.id}
          renderItem={({ item }) => (
            <View style={styles.resultRow}>
              <Text style={styles.resultCell}>#{item.level}</Text>
              <Text style={styles.resultCell}>{item.price}</Text>
            </View>
          )}
        />
      )}
    </View>
  </View>

  <Text style={styles.footer}>جميع الحقوق محفوظة — HUSSEIN_ROYAL ©</Text>
</SafeAreaView>

); }

const styles = StyleSheet.create({ container: { flex: 1, backgroundColor: "#0b0b0f", alignItems: "center", padding: 20 }, title: { fontSize: 28, fontWeight: "900", color: "#ffcc00", marginTop: 20 }, subtitle: { fontSize: 14, color: "#aaa", marginBottom: 20 }, card: { backgroundColor: "#111217", borderRadius: 16, padding: 20, width: "100%" }, label: { color: "#fff", marginTop: 10, marginBottom: 4, fontWeight: "700" }, input: { backgroundColor: "#0b0c11", color: "#fff", padding: 12, borderRadius: 10, marginBottom: 10 }, row: { flexDirection: "row", gap: 10 }, option: { flex: 1, padding: 12, borderRadius: 10, backgroundColor: "#222", alignItems: "center" }, activeOption: { backgroundColor: "#ffcc00" }, optionText: { color: "#fff", fontWeight: "700" }, switchRow: { flexDirection: "row", alignItems: "center", marginVertical: 10 }, switchLabel: { color: "#fff", marginLeft: 10 }, button: { backgroundColor: "#ffcc00", padding: 14, borderRadius: 12, marginTop: 10, alignItems: "center" }, buttonText: { fontWeight: "900", fontSize: 16 }, results: { marginTop: 20, backgroundColor: "#0c0d12", borderRadius: 12, padding: 10, minHeight: 80 }, muted: { color: "#888", textAlign: "center" }, resultRow: { flexDirection: "row", justifyContent: "space-between", paddingVertical: 6, borderBottomWidth: 1, borderBottomColor: "#222" }, resultCell: { color: "#fff" }, footer: { color: "#777", fontSize: 12, marginTop: 20 }, });

