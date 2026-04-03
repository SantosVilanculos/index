```sh

    def _compare_q_file_info(self, item1: str, item2: str, q_string: str) -> int:
        index1 = item1.lower().find(q_string.lower())
        index2 = item2.lower().find(q_string.lower())

        if index1 < index2:
            return -1
        elif index1 > index2:
            return 1
        else:
            return 0

    def _get_sorted_data(self, data: List[str], q_string: str) -> List[str]:
        f1 = list(filter(lambda item: q_string.lower() in item.lower(), data))
        f2 = sorted(
            f1,
            key=cmp_to_key(
                lambda item1, item2: self._compare_q_file_info(item1, item2, q_string)
            ),
        )
        return f2
```
