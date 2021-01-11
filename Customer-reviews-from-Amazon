import csv
import io
import re
from nltk.tokenize import sent_tokenize
import nltk
nltk.download('punkt')

def csv_dict_reader(file_name: str) -> list:
    with io.open(file_name, encoding='utf-8') as file_obj:
        reader = csv.DictReader(file_obj, delimiter=',')
        reviews = []
        for c, line in enumerate(reader, start=1):
            reviews.append(line["customer_reviews"])
            if c == 1000:
                break

        return reviews


def clean_names(reviews: list) -> list:
    pattern = r"\s{4}\n\s{4}(.*)\n\s{2}\n\s*"
    reviews_without_names = []
    for review in reviews[:1000]:
        review_without_name = re.sub(pattern, "", review)
        reviews_without_names.append(review_without_name)

    return reviews_without_names


def split_reviews(reviews_without_names: list) -> list:
    splitted_reviews = []
    for r in reviews_without_names:
        pattern = r"(if\(ue\) {)(?<=(if\(ue\) {))[\n*\D*\d*.*]*(?=Mins)Mins"
        r = re.sub(pattern, "", r).strip()

        splitted_reviews.extend(r.split('\n'))

    return splitted_reviews


def cleaning(splitted_reviews: list) -> list:
    first_reg = r"(// \d.\d // [^/]+ //)( \d+ of \d+ people found the following review helpful //)?( By$)?"
    second_reg = r".*on \d+ \D* \d{4} //"

    cleaned_reviews = []
    for r in splitted_reviews:
        r = re.sub(first_reg, "", r).strip()
        r = re.sub(second_reg, "", r).strip()

        if ' | ' in r:
            if re.search("\. \| ", r):
                r = r.replace(' | ', ' ')
            else:
                r = r.replace(' | ', '. ')
        if r:
            cleaned_reviews.append(r)

    return cleaned_reviews


def splited_to_sentences(cleaned_reviews: list) -> str:
    joined_reviews = "\n".join(cleaned_reviews)
    sentences = "\n".join(sent_tokenize(joined_reviews))

    return sentences


def writing_to_file(sentences: str) -> None:
    with open('cleaned_customer_reviews.txt', 'w', encoding='UTF-8') as f:
        f.write(sentences)


def main():
    print('Start...')

    file_name = "amazon_co-ecommerce_sample.csv"
    reviews = csv_dict_reader(file_name)
    reviews_without_names = clean_names(reviews)
    splitted_reviews = split_reviews(reviews_without_names)
    cleaned_reviews = cleaning(splitted_reviews)
    sentences = splited_to_sentences(cleaned_reviews)
    writing_to_file(sentences)

    print('Done. Sentences are saved to file \"cleaned_customer_reviews.txt"')


if __name__ == "__main__":
    main()

