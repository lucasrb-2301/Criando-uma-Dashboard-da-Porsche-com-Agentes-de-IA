# Criando-uma-Dashboard-da-Porsche-com-Agentes-de-IA

class PorscheAgent {
  constructor(rawText) {
    this.rawText = rawText;
    this.data = this.parse(rawText);
  }

  // Converte o texto em registros estruturados
  parse(text) {
    const lines = text.split("\n").map(l => l.trim()).filter(l => l.length > 0);

    const records = [];
    let buffer = [];

    for (const line of lines) {
      // Cada registro tem 14 campos
      buffer.push(line);

      if (buffer.length === 14) {
        const [
          sale_id,
          sale_date,
          customer_name,
          model,
          year,
          price,
          mileage,
          pay_method,
          city_raw,
          city,
          state,
          salesperson,
          delivery_raw,
          delivery
        ] = buffer;

        // Ignorar linhas com sale_id = INVALID
        if (sale_id !== "INVALID") {
          records.push({
            sale_id: Number(sale_id),
            sale_date: sale_date === "INVALID" ? null : sale_date,
            customer_name,
            model,
            year: Number(year),
            price: Number(price),
            mileage: Number(mileage),
            pay_method,
            city,
            state,
            salesperson,
            delivery_status: delivery
          });
        }

        buffer = [];
      }
    }

    return records;
  }

  // Retorna todos os dados
  getAll() {
    return this.data;
  }

  // Filtra por qualquer campo
  filter(criteria = {}) {
    return this.data.filter(row =>
      Object.entries(criteria).every(([key, value]) =>
        value === "" || row[key] === value
      )
    );
  }

  // KPIs
  totalSales() {
    return this.data.length;
  }

  totalRevenue() {
    return this.data.reduce((sum, r) => sum + r.price, 0);
  }

  avgTicket() {
    return this.totalRevenue() / this.totalSales();
  }

  // Agrupamento genérico
  groupBy(field) {
    const map = {};
    for (const row of this.data) {
      map[row[field]] = map[row[field]] || [];
      map[row[field]].push(row);
    }
    return map;
  }

  // Receita por modelo
  revenueByModel() {
    const groups = this.groupBy("model");
    const result = {};
    for (const model in groups) {
      result[model] = groups[model].reduce((s, r) => s + r.price, 0);
    }
    return result;
  }

  // Vendas por cidade
  salesByCity() {
    const groups = this.groupBy("city");
    const result = {};
    for (const city in groups) {
      result[city] = groups[city].length;
    }
    return result;
  }

  // Métodos de pagamento
  payMethodUsage() {
    const groups = this.groupBy("pay_method");
    const result = {};
    for (const pay in groups) {
      result[pay] = groups[pay].length;
    }
    return result;
  }
}

// Instancia o agente usando o conteúdo do arquivo carregado
const agent = new PorscheAgent(`<?conteudo_do_arquivo?>`);

// Exemplos de uso:
console.log("Total de vendas:", agent.totalSales());
console.log("Receita total:", agent.totalRevenue());
console.log("Ticket médio:", agent.avgTicket());
console.log("Receita por modelo:", agent.revenueByModel());
console.log("Vendas por cidade:", agent.salesByCity());
console.log("Métodos de pagamento:", agent.payMethodUsage());
