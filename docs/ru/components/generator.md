# Генератор

Если вам нужно создать схему на основе набора параметров (скажем, резервуара из 100 случайно соединенных нейронов), вы можете использовать генераторы. По сути, генератор — это функция, которая принимает набор пользовательских параметров и возвращает список узлов схемы в формате json.

Генераторы имеют доступ к текущей сети через Engine API. Вот пример кода для генератора, создающего «резервуар» (набор случайным образом рекуррентно связанных нейронов с фиксированными синаптическими весами):

~~~js
import Generator from "@flux-agi/generator";

class ReservoirDefinition extends Generator {
  name = "Reservoir";
  version = "0.0.1";
  engine = "0.0.1";
  alias = "reservoir";

  getSettingsScheme = () => [
    {
      label: "title:",
      name: "title",
      type: String,
      default: this.name,
    },
    {
      label: "Number of neurons:",
      name: "numberOfNeurons",
      type: Number,
      default: 100,
    },
  ];

  generate = () => {
    const {
      numberOfNeurons,
      connectionsDensity,
      inputNeuronsNumber,
      outputNeuronsNumber,
    } = this.settings;
    const neuronsPerLine = Math.ceil(Math.sqrt(numberOfNeurons));
    const neurons = Array(numberOfNeurons).map((_, i: number) => {
      const x = i * 60;
      const y = Math.floor(i / neuronsPerLine) * 60;
      return {
        x,
        alias: `${this.alias}_neuron_${i}`,
        y,
        neuronType: "IAF",
        autoFiring: 0,
        threshold: 100,
        maxSum: 100,
        leak: 20,
        refractoryPeriod: 5,
        isOutput: true,
      };
    });

    return neurons;
  };
}

export default ReservoirDefinition;

~~~

Генератор — это, по сути, шаблон, который помогает вам создавать настраиваемые схемы на лету на основе набора параметров, которые вы предоставляете.
